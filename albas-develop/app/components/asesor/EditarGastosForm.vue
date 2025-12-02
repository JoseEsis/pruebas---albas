<script setup lang="ts">
import { ref, computed, watchEffect } from 'vue';

// Definición de Tipos para mayor claridad
interface Gasto {
  id: number | string;
  categoria: string;
  concepto: string;
  monto: string;
  fecha: string;
  notas: string;
}

interface Propiedad {
  id: number | string;
  nombre: string;
}

const props = defineProps<{
  /** La propiedad que ya tiene gastos asociados, si estamos editando. (Mockeado si no se usa) */
  propiedadInicial?: Propiedad | null;
  /** Los gastos que ya están guardados en el sistema para esta propiedad. */
  gastosIniciales?: Gasto[];
  /** Lista completa de propiedades disponibles para el selector. */
  propiedadesDisponibles?: Propiedad[];
}>();

const emit = defineEmits(["close", "guardar"]); 

// --- ESTADO ---
const propiedadSeleccionadaId = ref<number | string>(props.propiedadInicial?.id || "");

// Estado para controlar qué gasto se está editando actualmente (null si se está agregando uno nuevo)
const gastoEditandoId = ref<number | string | null>(null);

// Formulario reactivo para CUALQUIER gasto (nuevo o editado)
const form = ref({
  concepto: "",
  monto: "",
  categoria: "",
  fecha: new Date().toISOString().split('T')[0],
  notas: ""
});

// LISTA DE GASTOS (REACTIVA) - Contiene los datos que se enviarán al guardar
const listaGastos = ref<Gasto[]>([]);

// Mock de propiedades
const listaPropiedades = computed(() => props.propiedadesDisponibles || [
  { id: 1, nombre: 'Vista Hermosa' },
  { id: 2, nombre: 'Lote Centro' }
]);

// Inicializa la lista de gastos y la propiedad seleccionada al cargar
watchEffect(() => {
    // Carga los gastos iniciales o el mock de la imagen si no hay datos
    if (props.gastosIniciales && props.gastosIniciales.length > 0) {
        listaGastos.value = props.gastosIniciales.map(g => ({
            ...g,
            monto: Number(g.monto).toFixed(2),
        }));
    } else {
        // Mock que replica las 3 filas de la imagen para la demostración
        listaGastos.value = [
            { id: 1, categoria: 'Transporte', concepto: 'Visita de propiedad', monto: '40.00', fecha: '2025-11-16', notas: 'Ejemplo de nota 1' },
            { id: 2, categoria: 'Transporte', concepto: 'Visita de propiedad', monto: '40.00', fecha: '2025-11-16', notas: '' },
            { id: 3, categoria: 'Transporte', concepto: 'Visita de propiedad', monto: '40.00', fecha: '2025-11-16', notas: 'Ejemplo de nota 3' }
        ];
    }
    // Carga la propiedad inicial
    if (props.propiedadInicial) {
        propiedadSeleccionadaId.value = props.propiedadInicial.id;
    }
});


// --- FUNCIONES ---

/**
 * Carga los datos de un gasto de la tabla al formulario para su edición.
 * @param gasto Gasto a editar.
 */
const cargarGastoParaEditar = (gasto: Gasto) => {
    // 1. Establecer el ID del gasto que estamos editando
    gastoEditandoId.value = gasto.id;

    // 2. Cargar los datos del gasto al formulario
    form.value.concepto = gasto.concepto;
    form.value.monto = gasto.monto;
    form.value.categoria = gasto.categoria;
    form.value.fecha = gasto.fecha;
    form.value.notas = gasto.notas;
};

/**
 * Agrega un nuevo gasto a la lista o actualiza el gasto que se está editando.
 */
const agregarOActualizarGasto = () => {
  // 1. Validación (igual que antes)
  if (!form.value.concepto || !form.value.monto) {
    alert("Falta concepto o monto");
    return;
  }

  const gastoPayload: Gasto = {
    id: gastoEditandoId.value || Date.now() + Math.random(), // Usa el ID si existe, si no, crea uno nuevo
    categoria: form.value.categoria || 'General',
    concepto: form.value.concepto,
    monto: Number(form.value.monto).toFixed(2),
    fecha: form.value.fecha,
    notas: form.value.notas
  };

  if (gastoEditandoId.value) {
    // MODO EDICIÓN: Encuentra el índice y reemplaza el objeto
    const index = listaGastos.value.findIndex(g => g.id === gastoEditandoId.value);
    if (index !== -1) {
      listaGastos.value[index] = gastoPayload;
    }
    gastoEditandoId.value = null; // Sale del modo edición
  } else {
    // MODO NUEVO: Agrega al inicio
    listaGastos.value.unshift(gastoPayload);
  }

  // 3. Limpiar el formulario
  limpiarFormulario();
};

const limpiarFormulario = () => {
    form.value.concepto = "";
    form.value.monto = "";
    form.value.categoria = "";
    form.value.notas = "";
    form.value.fecha = new Date().toISOString().split('T')[0];
    gastoEditandoId.value = null; // Asegura que se desactive el modo edición
}

const eliminarGasto = (id: number | string) => {
  listaGastos.value = listaGastos.value.filter(g => g.id !== id);
  // Si eliminamos el gasto que se estaba editando, limpiamos el formulario
  if (gastoEditandoId.value === id) {
      limpiarFormulario();
  }
};

const guardarCambios = () => {
  if (!propiedadSeleccionadaId.value) {
    alert("Seleccione una propiedad.");
    return;
  }
  // Emitimos la lista de gastos para que el componente padre (Nuxt/Backend) la guarde
  emit("guardar", {
    propiedadId: propiedadSeleccionadaId.value,
    gastos: listaGastos.value
  });
  emit("close");
};

// Título del formulario (Agregar Nuevo Gasto vs. Editando Categoría)
const tituloFormulario = computed(() => {
    return gastoEditandoId.value ? 'Editando Categoría' : 'Agregar Nueva Categoría';
});
</script>

<template>
  <div 
    class="fixed inset-0 bg-black/60 backdrop-blur-sm z-40 transition-opacity"
    @click="$emit('close')"
  ></div>

  <div class="fixed inset-0 flex items-center justify-center z-50 pointer-events-none p-4">
    <div class="bg-white rounded-[24px] p-0 shadow-2xl w-full max-w-[900px] max-h-[90vh] overflow-hidden pointer-events-auto font-sans relative border border-gray-100 flex flex-col">
      
      <div class="px-8 py-6 border-b border-gray-100 flex justify-between items-center bg-white shrink-0">
        <div>
          <h2 class="text-3xl font-extrabold text-gray-900 tracking-tight">Editar Gastos</h2>
          <p class="text-xs text-gray-500 mt-1">Edite la información de gastos</p>
        </div>
        <button 
          class="text-gray-400 hover:text-black transition-colors bg-gray-50 hover:bg-gray-100 rounded-full p-2"
          @click="$emit('close')"
        >
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-5 h-5">
            <path stroke-linecap="round" stroke-linejoin="round" d="M6 18 18 6M6 6l12 12" />
          </svg>
        </button>
      </div>

      <div class="overflow-y-auto p-8 custom-scrollbar flex-grow">
        
        <div class="mb-8">
          <label class="block text-xs font-bold text-gray-500 uppercase mb-2 ml-1">Seleccione una propiedad</label>
          <div class="relative w-full max-w-sm">
            <select 
              v-model="propiedadSeleccionadaId"
              class="w-full appearance-none bg-[#E0E0E0] rounded-xl py-3 pl-4 pr-10 text-sm font-bold text-gray-800 focus:outline-none focus:ring-2 focus:ring-black/10 cursor-pointer shadow-inner"
            >
              <option value="" disabled selected>Seleccionar...</option>
              <option v-for="prop in listaPropiedades" :key="prop.id" :value="prop.id">
                {{ prop.nombre }}
              </option>
            </select>
            <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-4 text-black">
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-4 h-4">
                <path stroke-linecap="round" stroke-linejoin="round" d="m19.5 8.25-7.5 7.5-7.5-7.5" />
              </svg>
            </div>
          </div>
        </div>

        <div class="border border-gray-300 rounded-[24px] p-6 mb-8 relative bg-white shadow-sm">
          <h3 class="text-sm font-bold text-black mb-5 flex items-center gap-2">
            <span class="w-2 h-2 bg-black rounded-full"></span>
            {{ tituloFormulario }}
          </h3>
          
          <div class="grid grid-cols-1 md:grid-cols-2 gap-x-8 gap-y-5">
            <div class="space-y-4">
              <div>
                <label class="block text-xs font-bold text-black mb-1.5 ml-1">Concepto</label>
                <input v-model="form.concepto" type="text" placeholder="Descripción del gasto" class="w-full bg-[#E0E0E0] rounded-xl py-2.5 px-4 text-sm font-medium text-gray-800 focus:bg-white focus:outline-none focus:ring-2 focus:ring-black/10 border border-transparent transition-all shadow-inner">
              </div>
              <div>
                <label class="block text-xs font-bold text-black mb-1.5 ml-1">Monto</label>
                <div class="relative">
                  <span class="absolute left-3 top-1/2 -translate-y-1/2 text-gray-500 font-bold text-xs">$</span>
                  <input v-model="form.monto" type="number" placeholder="100.00" class="w-full bg-[#E0E0E0] rounded-xl py-2.5 pl-7 pr-4 text-sm font-medium text-gray-800 focus:bg-white focus:outline-none focus:ring-2 focus:ring-black/10 border border-transparent transition-all shadow-inner">
                </div>
              </div>
              <div>
                <label class="block text-xs font-bold text-black mb-1.5 ml-1">Notas</label>
                <input v-model="form.notas" type="text" placeholder="Notas adicionales" class="w-full bg-[#E0E0E0] rounded-xl py-2.5 px-4 text-sm font-medium text-gray-800 focus:bg-white focus:outline-none focus:ring-2 focus:ring-black/10 border border-transparent transition-all shadow-inner">
              </div>
            </div>

            <div class="space-y-4 relative flex flex-col">
              <div>
                <label class="block text-xs font-bold text-black mb-1.5 ml-1">Categoría</label>
                <input v-model="form.categoria" type="text" placeholder="Mantenimiento" class="w-full bg-[#E0E0E0] rounded-xl py-2.5 px-4 text-sm font-medium text-gray-800 focus:bg-white focus:outline-none focus:ring-2 focus:ring-black/10 border border-transparent transition-all shadow-inner">
              </div>
              <div>
                <label class="block text-xs font-bold text-black mb-1.5 ml-1">Fecha</label>
                <div class="relative">
                  <input v-model="form.fecha" type="date" class="w-full bg-[#E0E0E0] rounded-xl py-2.5 pl-4 pr-10 text-sm font-medium text-gray-800 focus:bg-white focus:outline-none focus:ring-2 focus:ring-black/10 border border-transparent transition-all shadow-inner">
                  <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-3 text-gray-500">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M6.75 3v2.25M17.25 3v2.25M3 18.75V7.5a2.25 2.25 0 0 1 2.25-2.25h13.5A2.25 2.25 0 0 1 21 7.5v11.25m-18 0A2.25 2.25 0 0 0 5.25 21h13.5A2.25 2.25 0 0 0 21 18.75m-18 0v-7.5A2.25 2.25 0 0 1 5.25 9h13.5A2.25 2.25 0 0 1 21 11.25v7.5" />
                    </svg>
                  </div>
                </div>
              </div>
              
              <div class="mt-auto flex justify-end pt-4">
                  <button 
                    @click="agregarOActualizarGasto"
                    :class="[
                        'text-white rounded-xl w-12 h-12 flex items-center justify-center transition-all shadow-lg active:scale-95',
                        gastoEditandoId ? 'bg-blue-600 hover:bg-blue-700' : 'bg-black hover:bg-gray-800'
                    ]"
                    :title="gastoEditandoId ? 'Actualizar Gasto' : 'Agregar a la lista'"
                  >
                    <svg v-if="!gastoEditandoId" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2.5" stroke="currentColor" class="w-6 h-6">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M12 4.5v15m7.5-7.5h-15" />
                    </svg>
                    <svg v-else xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-6 h-6">
                        <path stroke-linecap="round" stroke-linejoin="round" d="m16.862 4.487 1.687-1.688a1.875 1.875 0 1 1 2.652 2.652L6.832 19.82a4.5 4.5 0 0 1-1.897 1.13l-2.685.8.8-2.685a4.5 4.5 0 0 1 1.13-1.897L16.863 4.487Zm0 0L19.5 7.125" />
                    </svg>
                  </button>
              </div>
            </div>
          </div>
        </div>

        <div class="rounded-xl overflow-hidden border border-gray-200 shadow-md bg-white">
          <div class="grid grid-cols-[1.5fr_2fr_1fr_1fr_60px_60px] bg-black text-white px-4 py-3 text-[11px] font-bold uppercase tracking-widest text-center items-center">
            <span class="text-left pl-2">Categoría</span>
            <span class="text-left">Concepto</span>
            <span class="text-left">Monto</span>
            <span>Fecha</span>
            <span>Notas</span>
            <span class="text-right pr-2">Acción</span>
          </div>

          <div class="divide-y divide-gray-100 bg-white min-h-[100px]">
            <div v-if="listaGastos.length === 0" class="py-8 text-center text-gray-400 italic text-sm">
              No hay gastos agregados.
            </div>

            <div 
              v-for="gasto in listaGastos" 
              :key="gasto.id"
              class="grid grid-cols-[1.5fr_2fr_1fr_1fr_60px_60px] px-4 py-3.5 items-center text-center transition-colors border-b border-gray-50 last:border-0"
              :class="{ 'bg-blue-50/50 hover:bg-blue-50': gastoEditandoId === gasto.id, 'hover:bg-gray-50': gastoEditandoId !== gasto.id }"
            >
              <span class="text-left font-bold text-gray-800 pl-2 text-xs bg-gray-100 px-2 py-1 rounded w-fit">{{ gasto.categoria }}</span>
              <span class="text-left font-medium text-gray-600 text-sm truncate" :title="gasto.concepto">{{ gasto.concepto }}</span>
              <span class="text-left font-extrabold text-black text-sm">$ {{ gasto.monto }}</span>
              <span class="text-xs text-gray-500">{{ gasto.fecha }}</span>
              
              <div class="flex justify-center relative group">
                <svg v-if="gasto.notas" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5 text-gray-400 hover:text-black cursor-help">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M2.036 12.322a1.012 1.012 0 0 1 0-.639C3.423 7.51 7.36 4.5 12 4.5c4.638 0 8.573 3.007 9.963 7.178.07.207.07.431 0 .639C20.577 16.49 16.64 19.5 12 19.5c-4.638 0-8.573-3.007-9.963-7.178Z" />
                    <path stroke-linecap="round" stroke-linejoin="round" d="M15 12a3 3 0 1 1-6 0 3 3 0 0 1 6 0Z" />
                </svg>
                <span v-else class="text-gray-300">-</span>
                <div v-if="gasto.notas" class="absolute bottom-full mb-2 hidden group-hover:block w-40 p-2 bg-black text-white text-xs rounded shadow-lg z-10 text-left">
                  {{ gasto.notas }}
                </div>
              </div>

              <div class="flex justify-end pr-2 gap-2">
                <button 
                    @click="cargarGastoParaEditar(gasto)" 
                    class="text-gray-400 hover:text-blue-600 transition-colors" 
                    title="Editar"
                    :class="{'text-blue-600': gastoEditandoId === gasto.id}"
                 >
                   <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5">
                       <path stroke-linecap="round" stroke-linejoin="round" d="m16.862 4.487 1.687-1.688a1.875 1.875 0 1 1 2.652 2.652L6.832 19.82a4.5 4.5 0 0 1-1.897 1.13l-2.685.8.8-2.685a4.5 4.5 0 0 1 1.13-1.897L16.863 4.487Zm0 0L19.5 7.125" />
                   </svg>
                 </button>
                <button @click="eliminarGasto(gasto.id)" class="text-gray-400 hover:text-red-600 transition-colors" title="Eliminar">
                  <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5">
                      <path stroke-linecap="round" stroke-linejoin="round" d="m14.74 9-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L18.16 19.673a2.25 2.25 0 0 1-2.244 2.077H8.084a2.25 2.25 0 0 1-2.244-2.077L4.772 5.79m14.456 0a48.108 48.108 0 0 0-3.478-.397m-12 .562c.34-.059.68-.114 1.022-.165m0 0a48.11 48.11 0 0 1 3.478-.397m7.5 0v-.916c0-1.18-.91-2.164-2.09-2.201a51.964 51.964 0 0 0-3.32 0c-1.18.037-2.09 1.022-2.09 2.201v.916m7.5 0a48.667 48.667 0 0 0-7.5 0" />
                  </svg>
                </button>
              </div>
            </div>
          </div>
        </div>

      </div>

      <div class="bg-gray-50 px-8 py-4 border-t border-gray-200 flex justify-end gap-3 shrink-0">
        <button 
          @click="$emit('close')"
          class="px-6 py-2.5 border border-gray-300 rounded-xl text-sm font-bold text-gray-700 bg-white hover:bg-gray-100 transition-colors shadow-sm"
        >
          Cancelar
        </button>
        <button 
          @click="guardarCambios"
          class="px-8 py-2.5 bg-black text-white rounded-xl text-sm font-bold hover:bg-gray-800 transition-all shadow-lg active:scale-95 flex items-center gap-2"
        >
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-4 h-4">
              <path stroke-linecap="round" stroke-linejoin="round" d="M16.5 3.75V16.5L12 14.25 7.5 16.5V3.75m9 0H18A2.25 2.25 0 0 1 20.25 6v12A2.25 2.25 0 0 1 18 20.25H6A2.25 2.25 0 0 1 3.75 18V6A2.25 2.25 0 0 1 6 3.75h1.5m9 0h-9" />
          </svg>
          Guardar Cambios
        </button>
      </div>

    </div>
  </div>
</template>

<style scoped>
/* Estilo limpio para el scroll */
.custom-scrollbar::-webkit-scrollbar { width: 6px; }
.custom-scrollbar::-webkit-scrollbar-track { background: #f9fafb; }
.custom-scrollbar::-webkit-scrollbar-thumb { background: #d1d5db; border-radius: 10px; }
.custom-scrollbar::-webkit-scrollbar-thumb:hover { background: #9ca3af; }

/* Ocultar el indicador de fecha predeterminado para usar el icono personalizado */
input[type="date"]::-webkit-calendar-picker-indicator {
  cursor: pointer; opacity: 0; position: absolute; right: 0; top: 0; bottom: 0; width: 40px;
}
</style>
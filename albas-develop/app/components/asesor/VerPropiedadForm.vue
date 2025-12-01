<script setup lang="ts">
// Importamos funciones reactivas de Vue
import { ref, computed, onMounted } from 'vue';

// Definimos las props que recibe este componente
const props = defineProps<{
  lead?: any;                     // Información del lead asociado (opcional)
  propiedadesDisponibles?: any[]; // Lista de propiedades enviada desde el componente padre
}>();

// Declaramos los eventos que este componente puede emitir al padre
const emit = defineEmits(["close"]);

// Propiedad seleccionada en el <select>
const propiedadSeleccionadaId = ref<number | string>("");

// Computed: si el padre no envía nada, devolvemos un array vacío
const listaPropiedades = computed(() => props.propiedadesDisponibles || []);

// Computed: obtiene la propiedad actualmente seleccionada
const propiedadActual = computed(() => {
  return listaPropiedades.value.find(p => p.id === propiedadSeleccionadaId.value) || {}; // Retorna {} si no encuentra nada
});

// Cuando se monta el componente, seleccionamos la primera propiedad disponible por defecto
onMounted(() => {
  if (listaPropiedades.value.length > 0) {
    propiedadSeleccionadaId.value = listaPropiedades.value[0].id; // Seleccionar automáticamente la primera
  }
});
</script>

<template>
  <!-- Fondo oscuro del modal -->
  <div 
    class="fixed inset-0 bg-black/60 backdrop-blur-sm z-40 transition-opacity"
    @click="$emit('close')"                      
  ></div>

  <!-- Contenedor principal del modal -->
  <div class="fixed inset-0 flex items-center justify-center z-50 pointer-events-none p-4">
    <div class="bg-white rounded-[24px] shadow-2xl w-full max-w-[750px] max-h-[90vh] overflow-y-auto pointer-events-auto font-sans relative flex flex-col">
      
      <!-- Encabezado -->
      <div class="flex justify-between items-start p-8 pb-0">
        <div>
          <h2 class="text-2xl font-bold text-gray-900">Detalles de la Propiedad</h2>
          <p class="text-sm text-gray-500 mt-1">Información registrada en el sistema.</p>
        </div>

        <!-- Botón para cerrar -->
        <button 
          class="text-gray-400 hover:text-black transition-colors bg-gray-50 hover:bg-gray-100 p-2 rounded-full"
          @click="$emit('close')"                
        >
          <!-- Icono de cierre -->
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-5 h-5">
            <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
          </svg>
        </button>
      </div>

      <!-- Contenido interno del modal -->
      <div class="p-8 pt-6 space-y-8">
        
        <!-- Mostrar solo si hay propiedad seleccionada -->
        <div v-if="propiedadActual.id">
          
          <!-- Selector de propiedades -->
          <div class="relative mb-8">
            <label class="block text-xs font-bold text-gray-500 uppercase tracking-wider mb-2 ml-1">
              Propiedad Visualizada
            </label>

            <div class="relative">
              <select 
                v-model="propiedadSeleccionadaId"     
                class="w-full appearance-none bg-gray-100 border border-transparent hover:border-gray-300 rounded-xl py-3 pl-4 pr-10 text-base font-bold text-gray-900 focus:ring-0 focus:border-black transition-all cursor-pointer"
              >
                <!-- Opciones del select -->
                <option v-for="prop in listaPropiedades" :key="prop.id" :value="prop.id">
                  {{ prop.nombre }}
                </option>
              </select>

              <!-- Flecha decorativa del select -->
              <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-4 text-gray-600">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" class="w-5 h-5">
                  <path fill-rule="evenodd" d="M5.23 7.21a.75.75 0 011.06.02L10 11.168l3.71-3.938a.75.75 0 111.08 1.04l-4.25 4.5a.75.75 0 01-1.08 0l-4.25-4.5a.75.75 0 01.02-1.06z" clip-rule="evenodd" />
                </svg>
              </div>
            </div>
          </div>

          <!-- Panel de información general -->
          <div class="rounded-2xl overflow-hidden border border-gray-200 shadow-sm mb-8">
            
            <!-- Título de sección -->
            <div class="bg-black py-4 px-6 flex items-center gap-3">
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5 text-white/80">
                <path stroke-linecap="round" stroke-linejoin="round" d="M11.25 11.25l.041-.02a.75.75 0 011.063.852l-.708 2.836a.75.75 0 001.063.853l.041-.021M21 12a9 9 0 11-18 0 9 9 0 0118 0zm-9-3.75h.008v.008H12V8.25z" />
              </svg>
              <h3 class="text-xs font-bold text-white uppercase tracking-widest">Información General</h3>
            </div>

            <!-- Cuerpo de información general -->
            <div class="bg-white p-6">
              
              <!-- Fila principal: ubicación + precio -->
              <div class="flex flex-col md:flex-row justify-between items-start gap-6 mb-6 border-b border-gray-100 pb-6">
                
                <!-- Columna izquierda: ubicación -->
                <div class="flex items-start gap-3 w-full">

                  <!-- Icono -->
                  <div class="text-gray-400 mt-1 flex-shrink-0">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
                      <path stroke-linecap="round" stroke-linejoin="round" d="M15 10.5a3 3 0 1 1-6 0 3 3 0 0 1 6 0Z" />
                      <path stroke-linecap="round" stroke-linejoin="round" d="M19.5 10.5c0 7.142-7.5 11.25-7.5 11.25S4.5 17.642 4.5 10.5a7.5 7.5 0 1 1 15 0Z" />
                    </svg>
                  </div>

                  <!-- Texto de ubicación -->
                  <div class="flex-1">
                    <p class="text-[10px] font-bold text-gray-400 uppercase tracking-wider mb-1">Ubicación</p>
                    <p class="text-lg font-bold text-gray-900 leading-snug break-words">
                      {{ propiedadActual.ubicacion || 'Sin ubicación registrada' }}
                    </p>
                  </div>

                </div>

                <!-- Columna derecha: precio -->
                <div class="text-left md:text-right flex-shrink-0 bg-gray-50 px-4 py-2 rounded-xl border border-gray-100">
                  <p class="text-[10px] font-bold text-gray-400 uppercase tracking-wider mb-1">Precio</p>

                  <!-- Cantidad -->
                  <div class="text-2xl font-extrabold text-black tracking-tight whitespace-nowrap">
                    <span class="text-lg text-gray-500 font-medium mr-1">$</span>{{ propiedadActual.precio || '0.00' }}
                  </div>
                </div>
              </div>

              <!-- Segunda fila: tamaño y documento -->
              <div class="grid grid-cols-2 md:grid-cols-2 gap-y-6 gap-x-4">
                
                <!-- Tamaño total -->
                <div class="flex flex-col">
                  <p class="text-[10px] font-bold text-gray-400 uppercase tracking-wider mb-1">Tamaño Total</p>

                  <div class="flex items-center gap-2">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-4 h-4 text-gray-400">
                      <path stroke-linecap="round" stroke-linejoin="round" d="M3.75 3.75v4.5m0-4.5h4.5m-4.5 0L9 9M3.75 20.25v-4.5m0 4.5h4.5m-4.5 0L9 15M20.25 3.75h-4.5m4.5 0v4.5m0-4.5L15 9m5.25 11.25h-4.5m4.5 0v-4.5m0 4.5L15 15" />
                    </svg>
                    <p class="text-sm font-bold text-gray-800 break-words">{{ propiedadActual.tamano || '0' }} m²</p>
                  </div>
                </div>

                <!-- Documento / partida -->
                <div class="flex flex-col">
                  <p class="text-[10px] font-bold text-gray-400 uppercase tracking-wider mb-1">Documento / Partida</p>

                  <div class="flex items-start gap-2">
                    <!-- Icono -->
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-4 h-4 text-gray-400 mt-0.5">
                      <path stroke-linecap="round" stroke-linejoin="round" d="M19.5 14.25v-2.625a3.375 3.375 0 0 0-3.375-3.375h-1.5A1.125 1.125 0 0 1 13.5 7.125v-1.5a3.375 3.375 0 0 0-3.375-3.375H8.25m0 12.75h7.5m-7.5 3H12M10.5 2.25H5.625c-.621 0-1.125.504-1.125 1.125v17.25c0 .621.504 1.125 1.125 1.125h12.75c.621 0 1.125-.504 1.125-1.125V11.25a9 9 0 0 0-9-9Z" />
                    </svg>

                    <!-- Valor del documento -->
                    <p class="text-sm font-bold text-gray-800 break-words leading-tight">
                      {{ propiedadActual.partida || propiedadActual.documento || 'No registrado' }}
                    </p>
                  </div>
                </div>

              </div>
            </div>
          </div>

          <!-- Panel de detalles adicionales -->
          <div class="rounded-2xl overflow-hidden border border-gray-200 shadow-sm">

            <!-- Título -->
            <div class="bg-black py-4 px-6 flex items-center gap-3">
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5 text-white/80">
                <path stroke-linecap="round" stroke-linejoin="round" d="M8.25 6.75h12M8.25 12h12m-12 5.25h12M3.75 6.75h.007v.008H3.75V6.75Zm.375 0a.375.375 0 1 1-.75 0 .375.375 0 0 1 .75 0ZM3.75 12h.007v.008H3.75V12Zm.375 0a.375.375 0 1 1-.75 0 .375.375 0 0 1 .75 0ZM3.75 17.25h.007v.008H3.75v-.008Zm.375 0a.375.375 0 1 1-.75 0 .375.375 0 0 1 .75 0Z" />
              </svg>
              <h3 class="text-xs font-bold text-white uppercase tracking-widest">Detalles Adicionales</h3>
            </div>

            <!-- Si existen detalles -->
            <div v-if="propiedadActual.detalles && propiedadActual.detalles.length > 0" class="bg-white">
              
              <!-- Encabezado de tabla -->
              <div class="grid grid-cols-[1fr_2fr] bg-gray-50 border-b border-gray-200 px-6 py-3">
                <span class="text-[10px] font-bold text-gray-400 uppercase tracking-wider">Característica</span>
                <span class="text-[10px] font-bold text-gray-400 uppercase tracking-wider">Descripción</span>
              </div>

              <!-- Contenido de tabla -->
              <div class="divide-y divide-gray-100">
                <div 
                  v-for="(detalle, index) in propiedadActual.detalles" 
                  :key="index"                
                  class="grid grid-cols-[1fr_2fr] px-6 py-4 items-start hover:bg-gray-50 transition-colors"
                >
                  <span class="text-sm font-bold text-gray-900 break-words pr-4">
                    {{ detalle.nombre }}
                  </span>
                  <span class="text-sm font-medium text-gray-600 break-words">
                    {{ detalle.valor }}
                  </span>
                </div>
              </div>
            </div>

            <!-- Si no hay detalles -->
            <div v-else class="text-center py-10 bg-white">
              <p class="text-sm text-gray-400 italic">No hay detalles adicionales registrados.</p>
            </div>
          </div>

        </div>

        <!-- Si no existe la propiedad -->
        <div v-else class="text-center py-12">
          <p class="text-gray-500 font-medium">No se encontró información de la propiedad.</p>
        </div>

      </div>

      <!-- Footer con botón cerrar -->
      <div class="flex justify-end p-8 pt-0">
        <button 
          @click="$emit('close')"                
          class="px-8 py-3 bg-black text-white rounded-xl text-sm font-bold hover:bg-gray-800 transition-all shadow-lg hover:shadow-xl active:scale-95 w-full md:w-auto"
        >
          Cerrar Visualización
        </button>
      </div>

    </div>
  </div>
</template>

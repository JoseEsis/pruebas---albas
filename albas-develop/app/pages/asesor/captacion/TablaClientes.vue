<script setup lang="ts">
// Recibimos los datos desde el padre (captacion.vue)
defineProps<{
  clientes: any[];
  vendidoClientes: string[]; // Nota: En tu código original se llamaba 'VendidoClientes'
}>();

// Definimos los eventos hacia el padre
const emit = defineEmits([
  "abrir-ver-historial",
  "abrir-ver-terrenos",
  "abrir-subir-contrato",
  "abrir-ver-comentarios",
  "editar-lead-cliente",
  "eliminar-lead-cliente",
]);
</script>

<template>
  <div class="bg-blanco-primario p-4 rounded-xl shadow-xl overflow-x-auto">
    <div class="mb-4">
      <div class="relative">
        <svg
          class="absolute left-3 top-1/2 transform -translate-y-1/2 h-5 w-5 text-gray-400"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"
          />
        </svg>
        <input
          type="text"
          placeholder="Buscar por nombre, apellido..."
          class="w-full pl-10 pr-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
        />
      </div>
    </div>

    <div
      class="grid grid-cols-12 gap-x-4 py-3 px-4 text-sm font-semibold text-gray-600 border-b border-gray-200"
    >
      <span>N°</span>
      <span class="col-span-2 -ml-18">Nombre Completo</span>
      <span class="-ml-25">Contacto</span>
      <span class="-ml-22">Fecha</span>
      <span class="-ml-24">Tipo</span>
      <span class="-ml-24">Historial</span>
      <span class="-ml-20">Propiedades</span>
      <span class="-ml-15">Contrato</span>
      <span class="-ml-15">Vendido</span>
      <span class="-ml-12">Observaciones</span>
      <span>Acciones</span>
    </div>

    <div class="divide-y divide-gray-100">
      <div
        v-for="cliente in clientes"
        :key="cliente.id"
        class="grid grid-cols-12 gap-x-4 py-3 px-4 text-sm text-gray-800 items-center"
      >
        <span>{{ cliente.id }}</span>
        <span class="col-span-2 -ml-19">{{ cliente.nombre }}</span>
        <span class="-ml-26">{{ cliente.celular }}</span>
        <span class="-ml-25">{{ cliente.fecha }}</span>
        <span class="-ml-25">{{ cliente.tipo }}</span>

        <span class="flex items-center space-x-1 -ml-18">
          <button
            @click="$emit('abrir-ver-historial', cliente)"
            class="bg-blanco-primario rounded-full flex items-center justify-center w-6 h-6 text-negro-primario shrink-0 -ml-2"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="currentColor"
              class="size-6"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M12 6v6h4.5m4.5 0a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z"
              />
            </svg>
          </button>
        </span>

        <span class="flex items-center space-x-1 -ml-18">
          <button
            @click="$emit('abrir-ver-terrenos', cliente)"
            class="bg-blanco-primario rounded-full flex items-center justify-center w-6 h-6 text-negro-primario shrink-0 ml-4"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="currentColor"
              class="size-6"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="m11.25 11.25.041-.02a.75.75 0 0 1 1.063.852l-.708 2.836a.75.75 0 0 0 1.063.853l.041-.021M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Zm-9-3.75h.008v.008H12V8.25Z"
              />
            </svg>
          </button>
        </span>

        <span class="flex items-center space-x-1 -ml-18">
          <button
            @click="$emit('abrir-subir-contrato', cliente)"
            class="bg-blanco-primario rounded-full flex items-center justify-center w-6 h-6 text-negro-primario shrink-0 ml-7"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="currentColor"
              class="size-6"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M12 10.5v6m3-3H9m4.06-7.19-2.12-2.12a1.5 1.5 0 0 0-1.061-.44H4.5A2.25 2.25 0 0 0 2.25 6v12a2.25 2.25 0 0 0 2.25 2.25h15A2.25 2.25 0 0 0 21.75 18V9a2.25 2.25 0 0 0-2.25-2.25h-5.379a1.5 1.5 0 0 1-1.06-.44Z"
              />
            </svg>
          </button>
        </span>

        <span class="relative">
          <select v-model="cliente.Vendido" class="border rounded px-2 py-1 text-sm -ml-20">
            <option v-for="e in vendidoClientes" :key="e" :value="e">{{ e }}</option>
          </select>
        </span>

        <span class="flex items-center space-x-1 -ml-18">
          <button
            @click="$emit('abrir-ver-comentarios', cliente)"
            class="bg-blanco-primario rounded-full flex items-center justify-center w-6 h-6 text-negro-primario shrink-0 ml-15"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="currentColor"
              class="size-6"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M2.036 12.322a1.012 1.012 0 0 1 0-.639C3.423 7.51 7.36 4.5 12 4.5c4.638 0 8.573 3.007 9.963 7.178.07.207.07.431 0 .639C20.577 16.49 16.64 19.5 12 19.5c-4.638 0-8.573-3.007-9.963-7.178Z"
              />
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M15 12a3 3 0 1 1-6 0 3 3 0 0 1 6 0Z"
              />
            </svg>
          </button>
        </span>

        <span class="flex space-x-2">
          <button
            @click="$emit('editar-lead-cliente', cliente)"
            class="bg-blanco-primario rounded-full flex items-center justify-center w-6 h-6 text-negro-primario shrink-0 ml-1"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="currentColor"
              class="w-5.5 h-5.5"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="m16.862 4.487 1.687-1.688a1.875 1.875 0 1 1 2.652 2.652L10.582 16.07a4.5 4.5 0 0 1-1.897 1.13L6 18l.8-2.685a4.5 4.5 0 0 1 1.13-1.897l8.932-8.931Zm0 0L19.5 7.125M18 14v4.75A2.25 2.25 0 0 1 15.75 21H5.25A2.25 2.25 0 0 1 3 18.75V8.25A2.25 2.25 0 0 1 5.25 6H10"
              />
            </svg>
          </button>

          <button
            @click="$emit('eliminar-lead-cliente', cliente)"
            class="bg-blanco-primario rounded-full flex items-center justify-center w-6 h-6 text-negro-primario shrink-0"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="currentColor"
              class="w-5 h-5"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="m14.74 9-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L18.16 19.673a2.25 2.25 0 0 1-2.244 2.077H8.084a2.25 2.25 0 0 1-2.244-2.077L4.772 5.79m14.456 0a48.108 48.108 0 0 0-3.478-.397m-12 .562c.34-.059.68-.114 1.022-.165m0 0a48.11 48.11 0 0 1 3.478-.397m7.5 0v-.916c0-1.18-.91-2.164-2.09-2.201a51.964 51.964 0 0 0-3.32 0c-1.18.037-2.09 1.022-2.09 2.201v.916m7.5 0a48.667 48.667 0 0 0-7.5 0"
              />
            </svg>
          </button>
        </span>
      </div>
    </div>
  </div>
</template>

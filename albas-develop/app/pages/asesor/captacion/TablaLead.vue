<script setup lang="ts">
// Recibimos la data desde el padre
defineProps<{
  leads: any[];
  estados: string[];
}>();

// Definimos los eventos que se enviarán al padre
const emit = defineEmits([
  "abrir-propiedad",
  "editar-propiedad",
  "ver-propiedad",
  "abrir-visita",
  "editar-visita",
  "ver-visita",
  "abrir-observacion",
  "editar-observacion",
  "ver-observacion",
  "editar-lead",
  "eliminar-lead", // Agregado para el botón de eliminar
  "guardar-lead", // Agregado para el botón de guardar
]);
</script>

<template>
  <div class="bg-blanco-primario p-4 rounded-xl shadow-xl overflow-x-auto">
    <div
      class="grid grid-cols-12 gap-x-1 py-3 px-4 text-sm font-semibold text-gray-600 border-b border-gray-200"
    >
      <span class="mr-2">N°</span>
      <span class="col-span-2 -ml-15">Nombre Completo</span>
      <span class="-ml-18">Celular</span>
      <span class="-ml-15">Fecha</span>
      <span class="-ml-13">Tipo</span>
      <span class="-ml-14">Propiedad</span>
      <span class="-ml-7">Visita</span>
      <span class="-ml-7">Observación</span>
      <span class="ml-4">Estado</span>
      <span class="ml-7">Acciones</span>
      <span class="ml-11">Guardar</span>
    </div>

    <div class="divide-y divide-gray-100">
      <div
        v-for="lead in leads"
        :key="lead.id"
        class="grid grid-cols-12 gap-x-4 py-3 px-4 text-sm text-gray-800 items-center"
      >
        <span>{{ lead.id }}</span>
        <span class="col-span-2 -ml-16">{{ lead.nombre }}</span>
        <span class="-ml-22">{{ lead.celular }}</span>
        <span class="-ml-20">{{ lead.fecha }}</span>
        <span class="-ml-16">{{ lead.tipo }}</span>

        <span class="flex items-center space-x-1 -ml-18">
          <button
            @click="$emit('abrir-propiedad', lead)"
            class="bg-negro-primario rounded-full flex items-center justify-center w-6 h-6 text-blanco-primario shrink-0 -ml-1"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
              stroke-width="1.5"
              class="w-4 h-4"
            >
              <path stroke-linecap="round" stroke-linejoin="round" d="M12 4.5v15m7.5-7.5h-15" />
            </svg>
          </button>

          <button
            @click="$emit('editar-propiedad', lead)"
            class="bg-blanco-primario rounded-full flex items-center justify-center w-6 h-6 text-negro-primario shrink-0 ml-1"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
              stroke-width="1.5"
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
            @click="$emit('ver-propiedad', lead)"
            class="bg-blanco-primario rounded-full flex items-center justify-center w-6 h-6 text-negro-primario shrink-0 ml-1"
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

        <span class="flex items-center space-x-1 -ml-18">
          <button
            @click="$emit('abrir-visita', lead)"
            class="bg-negro-primario rounded-full flex items-center justify-center w-6 h-6 text-blanco-primario shrink-0 ml-5"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
              stroke-width="1.5"
              class="w-4 h-4"
            >
              <path stroke-linecap="round" stroke-linejoin="round" d="M12 4.5v15m7.5-7.5h-15" />
            </svg>
          </button>

          <button
            @click="$emit('editar-visita', lead)"
            class="bg-blanco-primario rounded-full flex items-center justify-center w-6 h-6 text-negro-primario shrink-0 ml-1"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
              stroke-width="1.5"
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
            @click="$emit('ver-visita', lead)"
            class="bg-blanco-primario rounded-full flex items-center justify-center w-6 h-6 text-negro-primario shrink-0 -ml-1"
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

        <span class="flex items-center space-x-1 -ml-18">
          <button
            @click="$emit('abrir-observacion', lead)"
            class="bg-negro-primario rounded-full flex items-center justify-center w-6 h-6 text-blanco-primario shrink-0 ml-9"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
              stroke-width="1.5"
              class="w-4 h-4"
            >
              <path stroke-linecap="round" stroke-linejoin="round" d="M12 4.5v15m7.5-7.5h-15" />
            </svg>
          </button>

          <button
            @click="$emit('editar-observacion', lead)"
            class="bg-blanco-primario rounded-full flex items-center justify-center w-6 h-6 text-negro-primario shrink-0 ml-1"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
              stroke-width="1.5"
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
            @click="$emit('ver-observacion', lead)"
            class="bg-blanco-primario rounded-full flex items-center justify-center w-6 h-6 text-negro-primario shrink-0 -ml-0"
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

        <span class="relative">
          <select v-model="lead.estado" class="border rounded px-2 py-1 text-sm -ml-6">
            <option v-for="e in estados" :key="e" :value="e">{{ e }}</option>
          </select>
        </span>

        <span class="flex space-x-2">
          <button
            @click="$emit('editar-lead', lead)"
            class="bg-blanco-primario rounded-full flex items-center justify-center w-6 h-6 text-negro-primario shrink-0 ml-7"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
              stroke-width="1.5"
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
            @click="$emit('eliminar-lead', lead)"
            class="bg-blanco-primario rounded-full flex items-center justify-center w-6 h-6 text-negro-primario shrink-0"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
              stroke-width="1.5"
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

        <span>
          <button
            @click="$emit('guardar-lead', lead)"
            class="bg-blanco-primario rounded-full flex items-center justify-center w-6 h-6 text-negro-primario ml-13"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
              stroke-width="1.5"
              class="w-5 h-5"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M3 7.5V6a2.25 2.25 0 0 1 2.25-2.25h10.5L21 8v10.5A2.25 2.25 0 0 1 18.75 20.75H5.25A2.25 2.25 0 0 1 3 18.75v-11.25Z"
              />
              <path stroke-linecap="round" stroke-linejoin="round" d="M16.5 3.75V8.25H7.5V3.75" />
              <path stroke-linecap="round" stroke-linejoin="round" d="M12 12.75v6" />
              <path stroke-linecap="round" stroke-linejoin="round" d="M9 15.75h6" />
            </svg>
          </button>
        </span>
      </div>
    </div>
  </div>
</template>

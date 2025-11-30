<script setup lang="ts">
import { ref } from "vue";


definePageMeta({
  layout: "asesor",
});

// Tabs
const currentTab = ref("Leads");
const setTab = (tab: string) => (currentTab.value = tab);

// Modal Lead
const showModal = ref(false);

// Formularios individuales nuevos Leads tabla leads
const showPropiedadForm = ref(false);
const showEditarPropiedadForm = ref(false);
const showVerPropiedadForm = ref(false);

const showVisitaForm = ref(false);
const showEditarVisitaForm = ref(false);
const showVerVisitaForm = ref(false);

const showObservacionForm = ref(false);
const showEditarObservacionForm = ref(false);
const showVerObservacionForm = ref(false);

const showEditarLeadForm = ref(false);

const selectedLead = ref<any>(null);

// Abrir formularios TABLA LEADS
const abrirPropiedadForm = (lead: any) => {
  selectedLead.value = lead;
  showPropiedadForm.value = true;
};

const abrirVisitaForm = (lead: any) => {
  selectedLead.value = lead;
  showVisitaForm.value = true;
};

const abrirObservacionForm = (lead: any) => {
  selectedLead.value = lead;
  showObservacionForm.value = true;
};

const abrirEditarPropiedadForm = (lead: any) => {
  selectedLead.value = lead;
  showEditarPropiedadForm.value = true;
};

const abrirVerPropiedadForm = (lead: any) => {
  selectedLead.value = lead;
  showVerPropiedadForm.value = true;
};

const abrirEditarVisitaForm = (lead: any) => {
  selectedLead.value = lead;
  showEditarVisitaForm.value = true;
};

const abrirVerVisitaForm = (lead: any) => {
  selectedLead.value = lead;
  showVerVisitaForm.value = true;
};

const abrirEditarObservacionForm = (lead: any) => {
  selectedLead.value = lead;
  showEditarObservacionForm.value = true;
};

const abrirVerObservacionForm = (lead: any) => {
  selectedLead.value = lead;
  showVerObservacionForm.value = true;
};

const abrirEditarLeadForm = (lead: any) => {
  selectedLead.value = lead;
  showEditarLeadForm.value = true;
};

// Datos Generados
const leads = ref([
  {
    id: 1,
    nombre: "Jose Mamani Lopez",
    celular: "945234123",
    fecha: "23 / 23 / 23",
    tipo: "Propios",
    estado: "Seguimiento",
  },
]);

// Guardar Lead
const guardarLead = (leadData: any) => {
  leads.value.push({
    id: leads.value.length + 1,
    estado: "Seguimiento",
    ...leadData,
  });

  showModal.value = false;
};

// Estados posibles
const estados = ["Seguimiento", "Cierre", "No responde"];
</script>

<template>
  <div class="p-8">
    <div class="flex justify-between items-center mb-6">
      <div class="flex space-x-2">
        <button
          @click="setTab('Leads')"
          class="px-4 py-2 rounded-full font-semibold transition-colors duration-150 shadow-md border border-gray-200"
          :class="{
            'bg-negro-primario text-white': currentTab === 'Leads',
            'bg-white text-negro-primario': currentTab !== 'Leads',
          }"
        >
          Leads
        </button>

        <button
          @click="setTab('Citas')"
          class="px-4 py-2 rounded-full font-semibold transition-colors duration-150 shadow-md border border-gray-200"
          :class="{
            'bg-negro-primario text-white': currentTab === 'Citas',
            'bg-white text-negro-primario': currentTab !== 'Citas',
          }"
        >
          Citas
        </button>

        <button
          @click="setTab('Clientes')"
          class="px-4 py-2 rounded-full font-semibold transition-colors duration-150 shadow-md border border-gray-200"
          :class="{
            'bg-negro-primario text-white': currentTab === 'Clientes',
            'bg-white text-negro-primario': currentTab !== 'Clientes',
          }"
        >
          Clientes
        </button>
      </div>

      <button
        @click="showModal = true"
        class="px-4 py-2 rounded-full font-semibold shadow-md bg-white text-negro-primario border border-gray-200"
      >
        + Agregar Lead
      </button>
    </div>

    <div v-if="currentTab === 'Leads'">
      <div
        class="rounded-[20px] shadow-xl overflow-hidden border border-gray-200 overflow-x-auto"
      >
        <div
          class="grid grid-cols-12 gap-x-1 py-4 px-4 text-sm font-bold text-gray-600 border-b border-gray-200 bg-[linear-gradient(180deg,#F9F9F9_10%,#EAEAEA_64%)]"
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
            class="grid grid-cols-12 gap-x-4 px-4 text-sm text-gray-800 items-center h-14 bg-[linear-gradient(180deg,#F9F9F9_10%,#EAEAEA_64%)]"
          >
            <span>{{ lead.id }}</span>
            <span class="col-span-2 -ml-16">{{ lead.nombre }}</span>
            <span class="-ml-22">{{ lead.celular }}</span>
            <span class="-ml-20">{{ lead.fecha }}</span>
            <span class="-ml-16">{{ lead.tipo }}</span>

            <span class="flex items-center space-x-1 -ml-18">
              <button
                @click="abrirPropiedadForm(lead)"
                class="bg-black rounded-full flex items-center justify-center w-6 h-6 text-white shrink-0 -ml-1"
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                  stroke-width="1.5"
                  class="w-4 h-4"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="M12 4.5v15m7.5-7.5h-15"
                  />
                </svg>
              </button>

              <button
                @click="abrirEditarPropiedadForm(lead)"
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
                @click="abrirVerPropiedadForm(lead)"
                class="bg-blanco-primario rounded-full flex items-center justify-center w-6 h-6 text-negro-primario shrink-0 ml-1"
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
                @click="abrirVisitaForm(lead)"
                class="bg-black rounded-full flex items-center justify-center w-6 h-6 text-white shrink-0 ml-5"
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                  stroke-width="1.5"
                  class="w-4 h-4"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="M12 4.5v15m7.5-7.5h-15"
                  />
                </svg>
              </button>

              <button
                @click="abrirEditarVisitaForm(lead)"
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
                @click="abrirVerVisitaForm(lead)"
                class="bg-blanco-primario rounded-full flex items-center justify-center w-6 h-6 text-negro-primario shrink-0 -ml-0"
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
                @click="abrirObservacionForm(lead)"
                class="bg-black rounded-full flex items-center justify-center w-6 h-6 text-white shrink-0 ml-9"
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                  stroke-width="1.5"
                  class="w-4 h-4"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="M12 4.5v15m7.5-7.5h-15"
                  />
                </svg>
              </button>

              <button
                @click="abrirEditarObservacionForm(lead)"
                class="bg-blanco-primario rounded-full fEditarObservacionFormlex items-center justify-center w-6 h-6 text-negro-primario shrink-0 ml-1"
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
                @click="abrirVerObservacionForm(lead)"
                class="bg-blanco-primario rounded-full flex items-center justify-center w-6 h-6 text-negro-primario shrink-0 -ml-0"
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
              <select
                v-model="lead.estado"
                class="border rounded px-2 py-1 text-sm -ml-6 bg-white/50"
              >
                <option v-for="e in estados" :key="e" :value="e">
                  {{ e }}
                </option>
              </select>
            </span>

            <span class="flex space-x-2">
              <button
                @click="abrirEditarLeadForm(lead)"
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

            <span>
              <button
                class="bg-blanco-primario rounded-full flex items-center justify-center w-6 h-6 text-negro-primario ml-13"
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
                    d="M3 7.5V6a2.25 2.25 0 0 1 2.25-2.25h10.5L21 8v10.5A2.25 2.25 0 0 1 18.75 20.75H5.25A2.25 2.25 0 0 1 3 18.75v-11.25Z"
                  />
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="M16.5 3.75V8.25H7.5V3.75"
                  />
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="M12 12.75v6"
                  />
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="M9 15.75h6"
                  />
                </svg>
              </button>
            </span>
          </div>
        </div>
      </div>
    </div>

    <div v-else-if="currentTab === 'Citas'">
      <div
        class="rounded-[20px] shadow-xl overflow-hidden border border-gray-200 overflow-x-auto"
      >
        <div
          class="grid grid-cols-10 gap-x-4 py-4 px-4 text-sm font-bold text-gray-600 border-b border-gray-200 bg-[linear-gradient(180deg,#F9F9F9_10%,#EAEAEA_64%)]"
        >
          <span>N°</span>
          <span class="col-span-2">Nombre Completo</span>
          <span>Celular</span>
          <span>Visitas</span>
          <span>Gastos</span>
          <span>Observaciones</span>
          <span>Estado</span>
          <span>Acciones</span>
          <span>Guardar</span>
        </div>

        <div class="divide-y divide-gray-100">
          <div
            v-for="n in 7"
            :key="'cita-' + n"
            class="grid grid-cols-10 gap-x-4 px-4 text-sm text-gray-800 items-center h-14 bg-[linear-gradient(180deg,#F9F9F9_10%,#EAEAEA_64%)]"
          >
            <span></span>
            <span class="col-span-2"></span>
            <span></span>
            <span></span>
            <span></span>
            <span></span>
            <span></span>
            <span></span>
            <span></span>
          </div>
        </div>
      </div>
    </div>

    <div v-else-if="currentTab === 'Clientes'">
      <div
        class="rounded-[20px] shadow-xl overflow-hidden border border-gray-200 overflow-x-auto"
      >
        <div class="mb-0 p-4 bg-white border-b border-gray-200">
          <div class="relative w-full max-w-md">
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
          class="grid grid-cols-11 gap-x-4 py-4 px-4 text-sm font-bold text-gray-600 border-b border-gray-200 bg-[linear-gradient(180deg,#F9F9F9_10%,#EAEAEA_64%)]"
        >
          <span class="col-span-2">Nombre Completo</span>
          <span>Contacto</span>
          <span>Fecha</span>
          <span>Tipo</span>
          <span>Historial</span>
          <span>Propiedades</span>
          <span>Contrato</span>
          <span>Vendido</span>
          <span>Observaciones</span>
          <span>Acciones</span>
        </div>

        <div class="divide-y divide-gray-100">
          <div
            v-for="n in 7"
            :key="'cliente-' + n"
            class="grid grid-cols-11 gap-x-4 px-4 text-sm text-gray-800 items-center h-14 bg-[linear-gradient(180deg,#F9F9F9_10%,#EAEAEA_64%)]"
          >
            <span class="col-span-2"></span>
            <span></span>
            <span></span>
            <span></span>
            <span></span>
            <span></span>
            <span></span>
            <span></span>
            <span></span>
            <span></span>
          </div>
        </div>
      </div>
    </div>

    <NuevoLeadForm
      v-if="showModal"
      @close="showModal = false"
      @crear="guardarLead"
    />

    <PropiedadForm
      v-if="showPropiedadForm"
      :lead="selectedLead"
      @close="showPropiedadForm = false"
    />

    <EditarPropiedadForm
      v-if="showEditarPropiedadForm"
      :lead="selectedLead"
      @close="showEditarPropiedadForm = false"
    />

    <VerPropiedadForm
      v-if="showVerPropiedadForm"
      :lead="selectedLead"
      @close="showVerPropiedadForm = false"
    />

    <VisitaForm
      v-if="showVisitaForm"
      :lead="selectedLead"
      @close="showVisitaForm = false"
    />

    <EditarVisitaForm
      v-if="showEditarVisitaForm"
      :lead="selectedLead"
      @close="showEditarVisitaForm = false"
    />

    <VerVisitaForm
      v-if="showVerVisitaForm"
      :lead="selectedLead"
      @close="showVerVisitaForm = false"
    />

    <ObservacionForm
      v-if="showObservacionForm"
      :lead="selectedLead"
      @close="showObservacionForm = false"
    />

    <EditarObservacionForm
      v-if="showEditarObservacionForm"
      :lead="selectedLead"
      @close="showEditarObservacionForm = false"
    />

    <VerObservacionForm
      v-if="showVerObservacionForm"
      :lead="selectedLead"
      @close="showVerObservacionForm = false"
    />

    <EditarLeadForm
      v-if="showEditarLeadForm"
      :lead="selectedLead"
      @close="showEditarLeadForm = false"
    />
  </div>
</template>

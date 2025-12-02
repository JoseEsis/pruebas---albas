<script setup lang="ts">
import { ref } from "vue";

/*---------------------------------------importaciones--------------------------------*/

//Import Tablas
import TablaLead from "~/pages/asesor/captacion/TablaLead.vue";
import TablaCitas from "~/pages/asesor/captacion/TablaCitas.vue";
import TablaClientes from "~/pages/asesor/captacion/TablaClientes.vue";

import NuevoLeadForm from "~/components/asesor/NuevoLeadForm.vue"; //Formulario para agregar Nuevo Lead de Captacion
//import leads-tabla-formularios

import PropiedadForm from "~/components/asesor/PropiedadForm.vue"; //Formulario para agregar Propiedad-tablaLeads
import EditarPropiedadForm from "~/components/asesor/EditarPropiedadForm.vue"; //Formulario para editar propiedad-tablaLeads
import VerPropiedadForm from "~/components/asesor/VerPropiedadForm.vue"; //Formulario para ver propiedad-tablaLeads
import VisitaForm from "~/components/asesor/VisitaForm.vue"; //Formulario para agregar Visitas-tablaLeads
import EditarVisitaForm from "~/components/asesor/EditarVisitaForm.vue"; //Formulario para editar visita-tablaLeads
import VerVisitaForm from "~/components/asesor/VerVisitaForm.vue"; //Formulario para ver visita-tablaLeads
import ObservacionForm from "~/components/asesor/NuevoLeadForm.vue"; //Formulario para agregar Observaciones-tablaLeads
import EditarObservacionForm from "~/components/asesor/NuevoLeadForm.vue"; //Formulario para editar observacion-tablaLeads
import VerObservacionForm from "~/components/asesor/NuevoLeadForm.vue"; //Formulario para ver observacion-tablaLeads
import EditarLeadForm from "~/components/asesor/NuevoLeadForm.vue"; //Formulario para editarLead-tablaLeads

//import citas-tabla-formularios

import VisitasForm from "~/components/asesor/NuevoLeadForm.vue"; //Formulario para agregarVisitas-tablaCitas
import EditarVisitasForm from "~/components/asesor/NuevoLeadForm.vue"; //Formulario para editarVisitas-tablaCitas
import VerVisitaAgendadaForm from "~/components/asesor/NuevoLeadForm.vue"; //Formulario para VerVisitas-tablaCitas
import GastosForm from "~/components/asesor/GastosForm.vue"; //Formulario para AgregarGastos-tablaCitas
import EditarGastosForm from "~/components/asesor/EditarGastosForm.vue"; //Formulario para EditarGastos-tablaCitas
import VerGastosForm from "~/components/asesor/VerGastosForm.vue"; //Formulario para VerGastos-tablaCitas
import ObservacionGastosForm from "~/components/asesor/NuevoLeadForm.vue"; //Formulario para agregarObservacion-tablaCitas
import EditarObservacionGastosForm from "~/components/asesor/NuevoLeadForm.vue"; //Formulario para EditarObservacion-tablaCitas
import VerObservacionGastosForm from "~/components/asesor/NuevoLeadForm.vue"; //Formulario para VerObservacion-tablaCitas
import EditarLeadCitaForm from "~/components/asesor/NuevoLeadForm.vue"; //Formulario para EditarLeadCita-tablaCitas

//import Clientes-tabla-formularios
import VerHistorialForm from "~/components/asesor/NuevoLeadForm.vue";
import VerTerrenosForm from "~/components/asesor/NuevoLeadForm.vue";
import SubirContratoForm from "~/components/asesor/NuevoLeadForm.vue";
import VerComentariosForm from "~/components/asesor/NuevoLeadForm.vue";
import EditarLeadClienteForm from "~/components/asesor/NuevoLeadForm.vue";

// Uso de diseño asesor
definePageMeta({
  layout: "asesor",
});

// =============================================================================
// 1. BASE DE DATOS SIMULADA (ARREGLOS)
// =============================================================================
// Aquí se guardarán los datos mientras la página esté abierta
const dbPropiedades = ref<any[]>([]);
const dbVisitas = ref<any[]>([]);
const dbObservaciones = ref<any[]>([]);

// Variables para pasar el dato específico a los modales
const selectedPropiedad = ref<any>(null);
const selectedVisita = ref<any>(null);
// Lista filtrada para cuando un lead tiene varias propiedades
const propiedadesDelLead = ref<any[]>([]);
const visitasDelLead = ref<any[]>([]);

// Tabs actual,(Leads, Citas, Clientes)
const currentTab = ref("Leads");
const setTab = (tab: string) => (currentTab.value = tab);

//Modal agregar Nuevo Lead
const showNuevoLeadForm = ref(false);

//---------------------------------------------------MOSTRAR MODAL LEADS--------------------------------------------------------------------

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

//Selector Leads
const selectedLead = ref<any>(null);

// =============================================================================
// LÓGICA DE PROPIEDADES (MODIFICADA PARA USAR BD SIMULADA)
// =============================================================================

// Abrir Modal Agregar
const abrirPropiedadForm = (lead: any) => {
  selectedLead.value = lead;
  showPropiedadForm.value = true;
};

// Guardar Nueva Propiedad
const guardarPropiedadBD = (datos: any) => {
  const nuevaPropiedad = { id: Date.now(), ...datos };
  dbPropiedades.value.push(nuevaPropiedad);
  console.log("Propiedad Guardada:", dbPropiedades.value);
  showPropiedadForm.value = false;
};

// Abrir Modal Editar (Busca si existe)
const abrirEditarPropiedadForm = (lead: any) => {
  selectedLead.value = lead;
  // Buscamos todas las propiedades de este lead
  const propsDelLead = dbPropiedades.value.filter((p) => p.leadId === lead.id);

  if (propsDelLead.length > 0) {
    propiedadesDelLead.value = propsDelLead; // Guardamos la lista para el select
    selectedPropiedad.value = propsDelLead[0]; // Seleccionamos la primera
    showEditarPropiedadForm.value = true;
  } else {
    alert("Primero debes AGREGAR una propiedad a este lead.");
  }
};

// Actualizar Propiedad
const actualizarPropiedadBD = (datos: any) => {
  const index = dbPropiedades.value.findIndex((p) => p.id === datos.id);
  if (index !== -1) {
    dbPropiedades.value[index] = datos;
  }
  showEditarPropiedadForm.value = false;
};

// Abrir Modal Ver
const abrirVerPropiedadForm = (lead: any) => {
  selectedLead.value = lead;
  const propsDelLead = dbPropiedades.value.filter((p) => p.leadId === lead.id);

  if (propsDelLead.length > 0) {
    propiedadesDelLead.value = propsDelLead;
    selectedPropiedad.value = propsDelLead[0];
    showVerPropiedadForm.value = true;
  } else {
    alert("No hay propiedad registrada para ver.");
  }
};

// =============================================================================
// LÓGICA DE VISITAS (MODIFICADA PARA USAR BD SIMULADA)
// =============================================================================

const abrirVisitaForm = (lead: any) => {
  selectedLead.value = lead;
  showVisitaForm.value = true;
};

const guardarVisitaBD = (datos: any) => {
  dbVisitas.value.push({ id: Date.now(), ...datos });
  showVisitaForm.value = false;
};

const abrirEditarVisitaForm = (lead: any) => {
  selectedLead.value = lead;
  const visita = dbVisitas.value.find((v) => v.leadId === lead.id);
  if (visita) {
    selectedVisita.value = visita;
    showEditarVisitaForm.value = true;
  } else {
    alert("No hay visita programada.");
  }
};

const actualizarVisitaBD = (datos: any) => {
  const index = dbVisitas.value.findIndex((v) => v.id === datos.id);
  if (index !== -1) dbVisitas.value[index] = datos;
  showEditarVisitaForm.value = false;
};

const abrirVerVisitaForm = (lead: any) => {
  selectedLead.value = lead;
  const visitaDelLead = dbVisitas.value.filter((v) => v.leadId === lead.id);
  if (visitaDelLead.length > 0) {
    visitasDelLead.value = visitaDelLead;
    selectedVisita.value = visitaDelLead[0];
    showVerVisitaForm.value = true;
  } else {
    alert("No hay visita registrada.");
  }
};

// =============================================================================
// RESTO DE FUNCIONES (MANTENIDAS IGUAL, SOLO PLACEHOLDERS)
// =============================================================================

const abrirObservacionForm = (lead: any) => {
  selectedLead.value = lead;
  showObservacionForm.value = true;
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

//---------------------------------------------------MOSTRAR MODAL Citas--------------------------------------------------------------------

const showVisitasForm = ref(false);
const showEditarVisitasForm = ref(false);
const showVerVisitaAgendadaForm = ref(false);

const showGastosForm = ref(false);
const showEditarGastosForm = ref(false);
const showVerGastosForm = ref(false);

const showObservacionGastosForm = ref(false);
const showEditarObservacionGastosForm = ref(false);
const showVerObservacionGastosForm = ref(false);

const showEditarLeadCitaForm = ref(false);

//Selector Citas
const selectedCita = ref<any>(null);

// Función temporal para modales que aun no tienen lógica de BD
const abrirModalTemporal = (item: any) => {
  console.log("Abrir modal (temporal)", item);
};

// Abrir formularios  Citas
const abrirVisitasForm = (cita: any) => {
  selectedCita.value = cita;
  showVisitasForm.value = true;
};
const abrirEditarVisitasForm = (cita: any) => {
  selectedCita.value = cita;
  showEditarVisitasForm.value = true;
};
const abrirVerVisitaAgendadaForm = (cita: any) => {
  selectedCita.value = cita;
  showVerVisitaAgendadaForm.value = true;
};

const abrirGastosForm = (cita: any) => {
  selectedCita.value = cita;
  showGastosForm.value = true;
};
const abrirEditarGastosForm = (cita: any) => {
  selectedCita.value = cita;
  showEditarGastosForm.value = true;
};
const abrirVerGastosForm = (cita: any) => {
  selectedCita.value = cita;
  showVerGastosForm.value = true;
};

const abrirObservacionGastosForm = (cita: any) => {
  selectedCita.value = cita;
  showObservacionGastosForm.value = true;
};
const abrirEditarObservacionGastosForm = (cita: any) => {
  selectedCita.value = cita;
  showEditarObservacionGastosForm.value = true;
};

const abrirVerObservacionGastosForm = (cita: any) => {
  selectedCita.value = cita;
  showVerObservacionGastosForm.value = true;
};

const abrirEditarLeadCitaForm = (cita: any) => {
  selectedCita.value = cita;
  showEditarLeadCitaForm.value = true;
};

//---------------------------------------------------MOSTRAR MODAL CLIENTES--------------------------------------------------------------------

const showVerHistorialForm = ref(false);
const showVerTerrenosForm = ref(false);
const showSubirContratoForm = ref(false);
const showVerComentariosForm = ref(false);
const showEditarLeadClienteForm = ref(false);

//Selector Clientes
const selectedCliente = ref<any>(null);

// Abrir formularios Clientes
const abrirVerHistorialForm = (cliente: any) => {
  selectedCliente.value = cliente;
  showVerHistorialForm.value = true;
};
const abrirVerTerrenosForm = (cliente: any) => {
  selectedCliente.value = cliente;
  showVerTerrenosForm.value = true;
};
const abrirSubirContratoForm = (cliente: any) => {
  selectedCliente.value = cliente;
  showSubirContratoForm.value = true;
};

const abrirVerComentariosForm = (cliente: any) => {
  selectedCliente.value = cliente;
  showVerComentariosForm.value = true;
};
const abrirEditarLeadClienteForm = (cliente: any) => {
  selectedCliente.value = cliente;
  showEditarLeadClienteForm.value = true;
};

//---------------------------------------------------DATOS GENERADOS(Adaptar)--------------------------------------------------------------------
// Datos Generados leads
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

//Datos Generados Citas
const citas = ref([
  {
    id: 1,
    nombre: "Jose Gonzales Lopez",
    celular: "999999999",
    fecha: "23 /24 / 23",
    tipo: "Propios",
    Estado: "realizado",
  },
]);

//Datos Generados Clientes
const clientes = ref([
  {
    id: 1,
    nombre: "Jose Vargas Lopez",
    celular: "888888888",
    fecha: "23 / 25 / 23",
    tipo: "Propios",
    Vendido: "seleccionar",
  },
]);

// Guardar Lead ESTADO
const guardarLead = (leadData: any) => {
  leads.value.push({
    id: leads.value.length + 1,
    estado: "Seguimiento",
    ...leadData,
  });
};

// Guardar Cita ESTADO
const guardarLeadCitas = (leadDataCitas: any) => {
  citas.value.push({
    id: citas.value.length + 1,
    Estado: "realizado",
    ...leadDataCitas,
  });
};

// Guardar Cliente ESTADO
const guardarLeadClientes = (leadDataClientes: any) => {
  clientes.value.push({
    id: clientes.value.length + 1,
    Vendido: "seleccionar",
    ...leadDataClientes,
  });
};

// ---------------Estados posibles----------------------------
const estados = ["Seguimiento", "Cierre", "No responde"];
const estadosCitas = ["realizado", "reprogramo", "cancelo"];
const vendidoClientes = ["seleccionar", "No", "Si"];
</script>

<template>
  <div class="p-8">
    <div class="flex justify-between items-center mb-6">
      <div class="flex space-x-2">
        <button
          @click="setTab('Leads')"
          class="px-4 py-2 rounded-full font-semibold transition-colors duration-150 shadow-md border border-gray-200"
          :class="
            currentTab === 'Leads'
              ? 'bg-negro-primario text-blanco-primario'
              : 'bg-blanco-primario text-negro-primario'
          "
        >
          Leads
        </button>

        <button
          @click="setTab('Citas')"
          class="px-4 py-2 rounded-full font-semibold transition-colors duration-150 shadow-md border border-gray-200"
          :class="
            currentTab === 'Citas'
              ? 'bg-negro-primario text-blanco-primario'
              : 'bg-blanco-primario text-negro-primario'
          "
        >
          Citas
        </button>

        <button
          @click="setTab('Clientes')"
          class="px-4 py-2 rounded-full font-semibold transition-colors duration-150 shadow-md border border-gray-200"
          :class="
            currentTab === 'Clientes'
              ? 'bg-negro-primario text-blanco-primario'
              : 'bg-blanco-primario text-negro-primario'
          "
        >
          Clientes
        </button>
      </div>

      <button
        @click="showNuevoLeadForm = true"
        class="px-4 py-2 rounded-full font-semibold shadow-md bg-blanco-primario text-negro-primario border border-gray-200"
      >
        + Agregar Lead
      </button>
    </div>

    <div v-if="currentTab === 'Leads'">
      <TablaLead
        :leads="leads"
        :estados="estados"
        @abrir-propiedad="abrirPropiedadForm"
        @editar-propiedad="abrirEditarPropiedadForm"
        @ver-propiedad="abrirVerPropiedadForm"
        @abrir-visita="abrirVisitaForm"
        @editar-visita="abrirEditarVisitaForm"
        @ver-visita="abrirVerVisitaForm"
        @abrir-observacion="abrirObservacionForm"
        @editar-observacion="abrirEditarObservacionForm"
        @ver-observacion="abrirVerObservacionForm"
        @editar-lead="abrirEditarLeadForm"
        @eliminar-lead="abrirModalTemporal"
        @guardar-lead="abrirModalTemporal"
      />
    </div>

    <div v-else-if="currentTab === 'Citas'">
      <TablaCitas
        :citas="citas"
        :estadosCitas="estadosCitas"
        @abrir-visitas="abrirVisitasForm"
        @editar-visitas="abrirEditarVisitasForm"
        @ver-visita-agendada="abrirVerVisitaAgendadaForm"
        @abrir-gastos="abrirGastosForm"
        @editar-gastos="abrirEditarGastosForm"
        @ver-gastos="abrirVerGastosForm"
        @abrir-observacion-gastos="abrirObservacionGastosForm"
        @editar-observacion-gastos="abrirEditarObservacionGastosForm"
        @ver-observacion-gastos="abrirVerObservacionGastosForm"
        @editar-lead-cita="abrirEditarLeadCitaForm"
        @eliminar-lead-cita="abrirModalTemporal"
        @guardar-lead-cita="abrirModalTemporal"
      />
    </div>

    <div v-else-if="currentTab === 'Clientes'">
      <TablaClientes
        :clientes="clientes"
        :vendidoClientes="vendidoClientes"
        @abrir-ver-historial="abrirVerHistorialForm"
        @abrir-ver-terrenos="abrirVerTerrenosForm"
        @abrir-subir-contrato="abrirSubirContratoForm"
        @abrir-ver-comentarios="abrirVerComentariosForm"
        @editar-lead-cliente="abrirEditarLeadClienteForm"
        @eliminar-lead-cliente="abrirModalTemporal"
        @guardar-lead-cliente="abrirModalTemporal"
      />
    </div>

    <NuevoLeadForm
      v-if="showNuevoLeadForm"
      @close="showNuevoLeadForm = false"
      @crear="guardarLead"
    />

    <PropiedadForm
      v-if="showPropiedadForm"
      :lead="selectedLead"
      @crear="guardarPropiedadBD"
      @close="showPropiedadForm = false"
    />

    <EditarPropiedadForm
      v-if="showEditarPropiedadForm"
      :lead="selectedLead"
      :propiedad="selectedPropiedad"
      :propiedadesDisponibles="propiedadesDelLead"
      @actualizar="actualizarPropiedadBD"
      @close="showEditarPropiedadForm = false"
    />

    <VerPropiedadForm
      v-if="showVerPropiedadForm"
      :lead="selectedLead"
      :propiedadesDisponibles="propiedadesDelLead"
      @close="showVerPropiedadForm = false"
    />

    <VisitaForm
      v-if="showVisitaForm"
      :lead="selectedLead"
      :propiedadesDisponibles="dbPropiedades"
      @crear="guardarVisitaBD"
      @close="showVisitaForm = false"
    />

    <EditarVisitaForm
      v-if="showEditarVisitaForm"
      :lead="selectedLead"
      :visita="selectedVisita"
      :propiedadesDisponibles="dbPropiedades"
      @actualizar="actualizarVisitaBD"
      @close="showEditarVisitaForm = false"
    />

    <VerVisitaForm
      v-if="showVerVisitaForm"
      :lead="selectedLead"
      :visitasDisponibles="visitasDelLead"
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

    <VisitasForm v-if="showVisitasForm" :cita="selectedCita" @close="showVisitasForm = false" />
    <EditarVisitasForm
      v-if="showEditarVisitasForm"
      :cita="selectedCita"
      @close="showEditarVisitasForm = false"
    />
    <VerVisitaAgendadaForm
      v-if="showVerVisitaAgendadaForm"
      :cita="selectedCita"
      @close="showVerVisitaAgendadaForm = false"
    />
    <GastosForm v-if="showGastosForm" :cita="selectedCita" @close="showGastosForm = false" />
    <EditarGastosForm
      v-if="showEditarGastosForm"
      :cita="selectedCita"
      @close="showEditarGastosForm = false"
    />
    <VerGastosForm
      v-if="showVerGastosForm"
      :cita="selectedCita"
      @close="showVerGastosForm = false"
    />
    <ObservacionGastosForm
      v-if="showObservacionGastosForm"
      :cita="selectedCita"
      @close="showObservacionGastosForm = false"
    />
    <EditarObservacionGastosForm
      v-if="showEditarObservacionGastosForm"
      :cita="selectedCita"
      @close="showEditarObservacionGastosForm = false"
    />
    <VerObservacionGastosForm
      v-if="showVerObservacionGastosForm"
      :cita="selectedCita"
      @close="showVerObservacionGastosForm = false"
    />
    <EditarLeadCitaForm
      v-if="showEditarLeadCitaForm"
      :cita="selectedCita"
      @close="showEditarLeadCitaForm = false"
    />

    <VerHistorialForm
      v-if="showVerHistorialForm"
      :cita="selectedCliente"
      @close="showVerHistorialForm = false"
    />
    <VerTerrenosForm
      v-if="showVerTerrenosForm"
      :cita="selectedCliente"
      @close="showVerTerrenosForm = false"
    />
    <SubirContratoForm
      v-if="showSubirContratoForm"
      :cita="selectedCliente"
      @close="showSubirContratoForm = false"
    />
    <VerComentariosForm
      v-if="showVerComentariosForm"
      :cita="selectedCliente"
      @close="showVerComentariosForm = false"
    />
    <EditarLeadClienteForm
      v-if="showEditarLeadClienteForm"
      :cita="selectedCliente"
      @close="showEditarLeadClienteForm = false"
    />
  </div>
</template>

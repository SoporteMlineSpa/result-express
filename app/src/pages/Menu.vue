<template>
  <q-page class="flex flex-center">
    <div class="q-pa-md">
      <q-stepper
        v-model="step"
        vertical
        color="primary"
        animated
        >

        <!-- Paso 1: Configuracion de Reportes -->
        <q-step
          :name="1"
          title="Paso 1: Configura tus reportes"
          active-color="deep-orange"
          icon="settings"
          :done="step > 1"
          >

          <q-btn class="q-pa-sm q-ma-sm" color="secondary" label="Clasificados" icon="fas fa-project-diagram" stack  @click="dClasificados = true"/>
            <q-btn class="q-pa-sm q-ma-sm" color="secondary" label="Unidad de Negocios" icon="fas fa-tags" stack @click="dUnidadNegocios = true"/>
              <q-btn class="q-pa-sm q-ma-sm" color="secondary" label="Centro de Costos" icon="fas fa-building" stack @click="dCentroCostos = true"/>

                <q-dialog full-width v-model="dClasificados">
                  <q-card>
                    <h4 class="text-center">Plan de Cuentas</h4>
                    <q-table
                      dense
                      :data="data"
                      :columns="[
                      { name: 'cuenta', align: 'center', label: 'Nivel Plan de Cuenta', field: 'nivel', sortable: true },
                      { name: 'cod', align: 'center', label: 'Codigo', field: 'cod', sortable: true },
                      { name: 'desc', align: 'center', label: 'Descripcion', field: 'desc', sortable: true },
                      { name: 'tipo', align: 'center', label: 'Tipo', field: 'tipo', sortable: true },
                      { name: 'auxiliar', align: 'center', label: 'Auxiliar', field: 'auxiliar', sortable: true },
                      { name: 'tipoIngreso', align: 'center', label: 'Tipo Ingreso/Costo', field: 'tipoIngreso', sortable: true },
                      { name: 'imputable', align: 'center', label: 'Imputable', field: 'imputable', sortable: true },
                      { name: 'negocio', align: 'center', label: 'Unidad de Negocios', field: 'negocio', sortable: true },
                      { name: 'costo', align: 'center', label: 'Centro de Costos', field: 'costo', sortable: true }
                      ]"
                      row-key="cod"
                      />
                  </q-card>
                </q-dialog>

                <q-dialog full-width v-model="dUnidadNegocios">
                  <q-card>
                    <div class="row items-center">
                      <q-card-section class="col-4">
                        <h4 class="text-center">Nueva Unidad de Negocio</h4>
                        <q-form
                          @submit="onSubmit($event)"
                          @reset="onReset"
                          class="q-gutter-md"
                          >
                          <q-input
                            filled
                            v-model="idUnidadNegocio"
                            label="ID"
                            hint="Identificador Unico"
                            />
                            <q-input
                              filled
                              v-model="nombreUnidadNegocio"
                              label="Nombre"
                              hint="Nombre de la unidad de negocio"
                              />
                              <div>
                                <q-btn label="Guardar" type="submit" color="primary" />
                                  <q-btn label="Limpiar" type="reset" color="primary" flat class="q-ml-sm" />
                              </div>
                        </q-form>
                      </q-card-section>
                      <q-card-section class="col">
                        <h4 class="text-center">Unidades de Negocios</h4>
                        <q-table
                          dense
                          :data="unidadNegocios"
                          :columns="
                          [
                          { name: 'id', align: 'center', label: 'ID', field: 'id', sortable: true },
                          { name: 'name', align: 'center', label: 'Nombre', field: 'nombre', sortable: true }
                          ]"
                          row-keys="id"/>
                      </q-card-section>
                    </div>
                  </q-card>
                </q-dialog>

                <q-dialog full-width v-model="dCentroCostos">
                  <q-card>
                    <div class="row items-center">
                      <q-card-section class="col-4">
                        <h4 class="text-center">Nuevo Centro de Costos</h4>
                        <q-form
                          @submit="onSubmit($event)"
                          @reset="onReset"
                          class="q-gutter-md"
                          >
                          <q-input
                            filled
                            v-model="idCentroCosto"
                            label="ID"
                            hint="Identificador Unico"
                            />
                            <q-input
                              filled
                              v-model="nombreCentroCosto"
                              label="Nombre"
                              hint="Nombre del centro de costo"
                              />

                              <div>
                                <q-btn label="Guardar" type="submit" color="primary" />
                                  <q-btn label="Limpiar" type="reset" color="primary" flat class="q-ml-sm" />
                              </div>
                        </q-form>
                      </q-card-section>
                      <q-card-section class="col">
                        <h4 class="text-center">Centros de Costo</h4>
                        <q-table
                          dense
                          :data="centroCostos"
                          :columns="
                          [
                          { name: 'id', align: 'center', label: 'ID', field: 'id', sortable: true },
                          { name: 'name', align: 'center', label: 'Nombre', field: 'nombre', sortable: true }
                          ]"
                          row-keys="id"/>
                      </q-card-section>
                    </div>
                  </q-card>
                </q-dialog>

                <q-stepper-navigation>
                  <q-btn @click="step = 2" color="primary" label="Continuar" />
                </q-stepper-navigation>
        </q-step>
        <!-- Paso 1: Configuracion de Reportes -->

        <!-- Paso 2: Cargar de Informacion -->
        <q-step
          :name="2"
          title="Paso 2: Cargar informacion"
          icon="create_new_folder"
          :done="step > 2"
          active-color="deep-orange"
          >
          <q-btn-group spread>
            <q-btn class="q-ma-sm q-pa-sm" icon="fas fa-sign-in-alt" color="green" label="Ventas" stack @click="dVentas = true"/>
              <q-btn class="q-ma-sm q-pa-sm" icon="fas fa-sign-out-alt" color="red" label="Compras" stack @click="dCompras = true"/>
                <q-btn class="q-ma-sm q-pa-sm" icon="fas fa-receipt" color="orange" label="Boleta de Honorarios" stack  @click="dBoletas = true"/>
          </q-btn-group>

          <q-dialog v-model="dCompras">
            <q-card>
              <q-card-section>
                <q-input
                  @input="val => { file = val[0] }"
                  filled
                  @change="reader($event, 2)"
                  type="file"
                  hint="Native file"
                  />
              </q-card-section>
              <q-card-section>
                <q-table
                  dense
                  :data="compras"
                  :columns="[
                  { name: 'nro', align: 'center', label: 'Nro', field: 'Nro', sortable: true },
                  { name: 'tipoDoc', align: 'center', label: 'Tipo Doc', field: 'Tipo Doc', sortable: true },
                  { name: 'rutProveedor', align: 'center', label: 'RUT Proveedor', field: 'RUT Proveedor', sortable: true },
                  { name: 'razonSocial', align: 'center', label: 'Razon Social', field: 'Razon Social', sortable: true },
                  { name: 'folio', align: 'center', label: 'Folio', field: 'Folio', sortable: true },
                  { name: 'fechaDcto', align: 'center', label: 'Fecha Documento', field: 'Fecha Docto', sortable: true },
                  { name: 'fechaRecp', align: 'center', label: 'Fecha Recepcion', field: 'Fecha Recepcion', sortable: true },
                  { name: 'montoExento', align: 'center', label: 'Monto Exento', field: 'Monto Exento', sortable: true },
                  { name: 'montoNeto', align: 'center', label: 'Monto Neto', field: 'Monto Neto', sortable: true },
                  { name: 'montoTotal', align: 'center', label: 'Monto Total', field: 'Monto Total', sortable: true },
                  ]"
                  row-keys="nro"/>
              </q-card-section>
            </q-card>
          </q-dialog>

          <q-dialog v-model="dVentas">
            <q-card>
              <q-card-section>
                <q-input
                  @input="val => { file = val[0] }"
                  filled
                  @change="reader($event, 1)"
                  type="file"
                  hint="Native file"
                  />
              </q-card-section>
              <q-card-section>
                <q-table
                  dense
                  :data="ventas"
                  :columns="[
                  { name: 'nro', align: 'center', label: 'Nro', field: 'Nro', sortable: true },
                  { name: 'tipoDoc', align: 'center', label: 'Tipo Doc', field: 'Tipo Doc', sortable: true },
                  { name: 'tipoVenta', align: 'center', label: 'Tipo Venta', field: 'Tipo Venta', sortable: true },
                  { name: 'rutCliente', align: 'center', label: 'RUT Cliente', field: 'Rut cliente', sortable: true },
                  { name: 'razonSocial', align: 'center', label: 'Razon Social', field: 'Razon Social', sortable: true },
                  { name: 'folio', align: 'center', label: 'Folio', field: 'Folio', sortable: true },
                  { name: 'fechaDcto', align: 'center', label: 'Fecha Documento', field: 'Fecha Docto', sortable: true },
                  { name: 'fechaRecp', align: 'center', label: 'Fecha Recepcion', field: 'Fecha Recepcion', sortable: true },
                  { name: 'montoExento', align: 'center', label: 'Monto Exento', field: 'Monto Exento', sortable: true },
                  { name: 'montoNeto', align: 'center', label: 'Monto Neto', field: 'Monto Neto', sortable: true },
                  { name: 'montoIva', align: 'center', label: 'Monto IVA', field: 'Monto IVA', sortable: true },
                  { name: 'montoTotal', align: 'center', label: 'Monto Total', field: 'Monto Total', sortable: true },
                  { name: 'codigoSucursal', align: 'center', label: 'Codigo Sucursal', field: 'Codigo Sucursal', sortable: true },
                  ]"
                  row-keys="nro"/>
              </q-card-section>
            </q-card>
          </q-dialog>

          <q-dialog v-model="dBoletas">
            <q-card>
              <q-card-section>
                <q-form
                  @submit="onSubmit"
                  @reset="onReset"
                  class="q-gutter-md">
                    <q-input
                      filled
                      v-model="nroBoleta"
                      label="Numero"/>
                    <q-input filled v-model="fechaBoleta" type="date" />
                    <q-input
                      filled
                      v-model="estadoBoleta"
                      label="Estado"/>
                    <q-input
                      filled
                      v-model="rutBoleta"
                      label="RUT Emisor"/>
                    <q-input
                      filled
                      v-model="nombreBoleta"
                      label="Nombre o Razon Social Emisor"/>
                    <q-input
                      filled
                      v-model="socProfBoleta"
                      label="Soc Prof"/>
                    <q-input
                      filled
                      v-model="brutoBoleta"
                      label="Honorario Bruto"/>
                    <q-input
                      filled
                      v-model="retenidoBoleta"
                      label="Honorario Retenido"/>
                    <q-input
                      filled
                      v-model="pagadoBoleta"
                      label="Honorario Pagado"/>
      <div>
        <q-btn label="Submit" type="submit" color="primary"/>
        <q-btn label="Reset" type="reset" color="primary" flat class="q-ml-sm" />
      </div>
                </q-form>
              </q-card-section>
              <q-card-section>
                <q-table
                  dense
                  :data="boletasHonorarios"
                  :columns="[
                  { name: 'nroBoleta', align: 'center', label: 'Numero', field: 'nro', sortable: true },
                  { name: 'fechaBoleta', align: 'center', label: 'Fecha', field: 'fecha', sortable: true },
                  { name: 'estadoBoleta', align: 'center', label: 'Estado', field: 'estado', sortable: true },
                  { name: 'rutBoleta', align: 'center', label: 'RUT Emisor', field: 'rut', sortable: true },
                  { name: 'nombreBoleta', align: 'center', label: 'Nombre Emisor', field: 'nombre', sortable: true },
                  { name: 'socProfBoleta', align: 'center', label: 'Soc. Prof.', field: 'socProf', sortable: true },
                  { name: 'brutoBoleta', align: 'center', label: 'Bruto', field: 'bruto', sortable: true },
                  { name: 'retenidoBoleta', align: 'center', label: 'Retenido', field: 'retenido', sortable: true },
                  { name: 'pagadoBoleta', align: 'center', label: 'Pagado', field: 'pagado', sortable: true },
                  ]"
                  row-key="nro"
                  />
              </q-card-section>
            </q-card>
          </q-dialog>

          <q-stepper-navigation>
            <q-btn @click="step = 3" color="primary" label="Continuar" />
              <q-btn flat @click="step = 1" color="primary" label="Regresar" class="q-ml-sm" />
          </q-stepper-navigation>
        </q-step>
        <!-- Paso 2: Cargar de Informacion -->

        <!-- Paso 3: Asignacion de Cuentas -->
        <q-step
          :name="3"
          title="Paso 3: Asignar Cuentas"
          icon="add_comment"
          :done="step > 3"
          active-color="deep-orange"
          >
          <q-stepper-navigation>
            <q-btn @click="step = 4" color="primary" label="Continuar" />
              <q-btn flat @click="step = 2" color="primary" label="Regresar" class="q-ml-sm" />
          </q-stepper-navigation>
        </q-step>
        <!-- Paso 3: Asignacion de Cuentas -->

        <!-- Paso 4: Mira tus reportes -->
        <q-step
          :name="4"
          title="Paso 4: Mira tus Reportes"
          icon="add_comment"
          active-color="deep-orange"
          >
          <q-stepper-navigation>
            <q-btn color="primary" label="Finalizar" />
              <q-btn flat @click="step = 3" color="primary" label="Regresar" class="q-ml-sm" />
          </q-stepper-navigation>
        </q-step>
        <!-- Paso 4: Mira tus reportes -->

      </q-stepper>
    </div>
  </q-page>
</template>

<script>
import json from '../assets/clasificados.json'
import XLSX from 'xlsx'

export default {
  data () {
    return {
      // Step inicial
      step: 1,

      // Booleans para dialogos
      dClasificados: false,
      dUnidadNegocios: false,
      dCentroCostos: false,
      dCompras: false,
      dVentas: false,
      dBoletas: false,

      // Variables
      idCentroCosto: null,
      nombreCentroCosto: null,
      idUnidadNegocio: null,
      nombreUnidadNegocio: null,
      centroCostos: [],
      unidadNegocios: [],
      ventas: [],
      compras: [],
      boletasHonorarios: [],
      nroBoleta: null,
      fechaBoleta: null,
      estadoBoleta: null,
      rutBoleta: null,
      nombreBoleta: null,
      socProfBoleta: null,
      brutoBoleta: null,
      retenidoBoleta: null,
      pagadoBoleta: null
    }
  },
  computed: {
    data: function () {
      return json
    }
  },
  methods: {
    onSubmit: function (evt) {
      if (this.idCentroCosto === null) {
        this.unidadNegocios.push(
          {
            'id': this.idUnidadNegocio,
            'nombre': this.nombreUnidadNegocio
          }
        )
      }

      if (this.idUnidadNegocio === null) {
        this.centroCostos.push(
          {
            'id': this.idCentroCosto,
            'nombre': this.nombreCentroCosto
          }
        )
      }

      if (this.idUnidadNegocio === null && this.idCentroCosto === null) {
        this.boletasHonorarios.push(
          {
            'nro': this.nroBoleta,
            'fecha': this.fechaBoleta,
            'estado': this.estadoBoleta,
            'rut': this.rutBoleta,
            'nombre': this.nombreBoleta,
            'socProf': this.socProfBoleta,
            'bruto': this.brutoBoleta,
            'retenido': this.retenidoBoleta,
            'pagado': this.pagadoBoleta
          }
        )
      }
      this.onReset()
    },
    onReset: function () {
      this.idCentroCosto = null
      this.nombreCentroCosto = null
      this.idUnidadNegocio = null
      this.nombreUnidadNegocio = null
      this.nroBoleta = null
      this.fechaBoleta = null
      this.estadoBoleta = null
      this.rutBoleta = null
      this.nombreBoleta = null
      this.socProfBoleta = null
      this.brutoBoleta = null
      this.retenidoBoleta = null
      this.pagadoBoleta = null
    },
    reader: function (evt, id) {
      var reader = new FileReader()
      var self = this
      reader.onload = function (e) {
        var data = new Uint8Array(e.target.result)
        var workbook = XLSX.read(data, { type: 'array' })
        var worksheet = workbook.Sheets[workbook.SheetNames[0]]
        if (id === 1) {
          self.ventas = XLSX.utils.sheet_to_json(worksheet)
        }
        if (id === 2) {
          self.compras = XLSX.utils.sheet_to_json(worksheet)
        }
      }
      reader.readAsArrayBuffer(evt.target.files[0])
    }
  }
}
</script>

<style lang="scss">
.table-responsive {
  overflow-y: hidden;
}
</style>

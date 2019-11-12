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
                      :columns="clasificados"
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
                          :data="unidadNegocios"
                          :columns="columns"
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
                          :data="centroCostos"
                          :columns="columns"
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

          <q-dialog v-model="dVentas">
            <q-card>
              <q-card-section>
                <q-input
                  @input="val => { file = val[0] }"
                  filled
                  type="file"
                  hint="Native file"
                />
              </q-card-section>
            </q-card>
          </q-dialog>

          <q-dialog v-model="dCompras">
            <q-card>
              <q-card-section>
                <q-input
                  @input="val => { file = val[0] }"
                  filled
                  type="file"
                  hint="Native file"
                />
              </q-card-section>
            </q-card>
          </q-dialog>

          <q-dialog v-model="dBoletas">
            <q-card>
              <q-card-section>
                <q-input
                  @input="val => { file = val[0] }"
                  filled
                  type="file"
                  hint="Native file"
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
      columns: [
        { name: 'id', align: 'center', label: 'ID', field: 'id', sortable: true },
        { name: 'name', align: 'center', label: 'Nombre', field: 'nombre', sortable: true }
      ],
      clasificados: [
        { name: 'cuenta', align: 'center', label: 'Nivel Plan de Cuenta', field: 'nivel', sortable: true },
        { name: 'cod', align: 'center', label: 'Codigo', field: 'cod', sortable: true },
        { name: 'desc', align: 'center', label: 'Descripcion', field: 'desc', sortable: true },
        { name: 'tipo', align: 'center', label: 'Tipo', field: 'tipo', sortable: true },
        { name: 'auxiliar', align: 'center', label: 'Auxiliar', field: 'auxiliar', sortable: true },
        { name: 'tipoIngreso', align: 'center', label: 'Tipo Ingreso/Costo', field: 'tipoIngreso', sortable: true },
        { name: 'imputable', align: 'center', label: 'Imputable', field: 'imputable', sortable: true },
        { name: 'negocio', align: 'center', label: 'Unidad de Negocios', field: 'negocio', sortable: true },
        { name: 'costo', align: 'center', label: 'Centro de Costos', field: 'costo', sortable: true }
      ]
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
        console.log(this.centroCostos)
      }
      this.onReset()
    },
    onReset: function () {
      this.idCentroCosto = null
      this.nombreCentroCosto = null
      this.idUnidadNegocio = null
      this.nombreUnidadNegocio = null
    }
  }
}
</script>

<style lang="scss">
  .table-responsive {
    overflow-y: hidden;
  }
</style>

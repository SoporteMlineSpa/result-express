<template>
  <q-page class="full-width row wrap justify-end  content-center">
    <q-img
      src="statics/office.jpg"
      class="header-image absolute-top"
    />
      <div class="q-pa-md q-mr-xl login-form shadow-3">
        <q-form
          @submit="onSubmit"
          class="q-gutter-md">

          <q-input
            v-model="user"
            label="Usuario"
            standout="bg-primary text-white"
            />

          <q-input
            v-model="password"
            label="Contraseña"
            type="password"
            standout="bg-primary text-white"
            />

            <div>
              <q-btn type="submit" label="Ingresar" color="primary" />
            </div>

        </q-form>

        <a href="">¿Olvidaste tu clave?</a>
        <a href="">¿No tienes una cuenta Rapido y Facil?</a>
      </div>

  </q-page>
</template>

<script>
export default {
  name: 'PageIndex'
}
</script>

<style lang="scss">
.header-image {
  height: 100%;
  z-index: -1;
  opacity: 0.8;
}

.login-form {
  background-color: rgba(255,255,255,0.8);
}

</style>

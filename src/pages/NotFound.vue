<template>
  <v-container class="d-flex flex-column justify-center align-center fill-height text-center">
    <v-row>
      <v-col cols="12">
        <v-icon
          size="96"
          color="red lighten-1"
        >
          mdi-emoticon-sad-outline
        </v-icon>
      </v-col>
    </v-row>

    <v-row>
      <v-col cols="12">
        <h1 class="display-1 font-weight-bold">
          404
        </h1>
        <p class="text-h5 text-secondary">
          Oops! The page you're looking for doesn't exist.
        </p>
      </v-col>
    </v-row>

    <v-row>
      <v-col cols="12">
        <v-btn
          color="primary"
          large
          @click="$router.push('/')"
        >
          <v-icon left>
            mdi-home
          </v-icon>
          Go Back to Home
        </v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>

</script>

<style scoped>
.fill-height {
  height: 100vh;
}
</style>

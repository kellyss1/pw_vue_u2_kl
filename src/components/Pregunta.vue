<template>
  <div>
    <img
      v-if="imagen"
      :src="imagen"
      alt="No se puede cargar la imagen"
    />
    <div class="oscuro">
      <div class="pregunta-container">
        <input
          v-model="pregunta"
          type="text"
          placeholder="Hazme una pregunta"
          name=""
          id=""
        />
        <p>Recuerda terminar con el signo de interrogación (?)</p>

        <h2>{{ pregunta }}</h2>
        <h1>{{ respuesta }}</h1>
      </div>
    </div>
  </div>
</template>

<script>
import { consumirAPIFacade } from "../clients/YesNoClient.js";

export default {
  data() {
    return {
      pregunta: null,
      respuesta: null,
      imagen: null,
    };
  },

  watch: {
    pregunta(value, oldValue) {
      //sync la propiedad reactiva con el input (ejemplo)
      //console.log(value);
      //console.log(oldValue);

      if (value.includes("?")) {
        this.consumir();
      }
    },
  },
  methods: {
    async consumir() {
      //llamar al API
      const resp = await consumirAPIFacade();
      console.log("Respuesta Final");
      console.log(resp);
      console.log(resp.answer);
      this.respuesta = resp.answer;
      this.imagen = resp.image;
    },
  },
};
</script>

<style>
.oscuro, img {
  height: 100vh;
  width: 100vw;
  max-height: 100%;
  max-width: 100%;
  position: fixed;
  left: 0px;
  top: 0px;
}

.pregunta-container {
  position: relative;

  min-height: 100vh; /* altura de toda la pantalla */
  display: flex;
  flex-direction: column; /* elementos uno debajo de otro */
  justify-content: center; /* centrado vertical */
  align-items: center; /* centrado horizontal */
}

input {
  width: 300px;
  padding: 10px 15px;
  border-radius: 5px;
  border: none;
  font-size: 16px;
}

input:focus {
  outline: none;
  box-shadow: 0 0 5px #42b983;
}

h1,
h2,
p {
  color: white;
}

p {
  margin-top: 20px;
  font-size: 20px;
}

h2 {
  margin-top: 40px;
  font-size: 30px;
  color: #42b983;
}

.oscuro {
  background-color: gray;
  z-index: 1;
}
</style>
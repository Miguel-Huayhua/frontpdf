<template>
  <div class="container">
  <h3>HOLA!!, BIENVENID@, ESTE MEDIO ES MI FORMA DE DARTE UN MENSAJE DE REGALO, ESPERO QUE TE GUSTE, GRACIAS.!!!</h3>
    <h5>Los campos irán a la carta, solo colocalo y genéralo...</h5>

    <form class="row mb-5" v-on:submit.prevent="submit" ref="form">
      <div class="col-12 col-md-8 mb-3">
        <div class="input-group">
          <span class="input-group-text">
            <label for="nombre" class="label-form">Nombre: </label>
          </span>
          <input
            class="form-control"
            name="nombre"
            id="nombre"
            type="text"
            placeholder="Introduzca su nombre"
          />
        </div>
      </div>

      <div class="col-12 col-md-8 mb-3">
        <div class="input-group">
          <span class="input-group-text">
            <label for="apellidos" class="label-form">Apellidos: </label>
          </span>
          <input
            class="form-control"
            name="apellido"
            id="apellidos"
            type="text"
            placeholder="Introduzca sus apellidos"
          />
        </div>
      </div>
       
      <input class="btn btn-secondary" type="submit" value="Generar Carta " />
    </form>
    <a href="https://regalonavidad.herokuapp.com" v-if="done"> Puedes descargarlo acá :)</a>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  components: {},
  data() {
    return {
      datos: false,
      archivo: "",
      done: false,
    };
  },
  methods: {
    dragleave(ev) {
      ev.preventDefault();
      this.$refs.file.classList.remove("file-enter");
    },
    dragenter(ev) {
      ev.preventDefault();
      this.$refs.file.classList.add("file-enter");
    },
    ondrop(ev) {
      ev.preventDefault();
      this.$refs.file.classList.remove("file-enter");
      console.log(ev.dataTransfer.files);
      if (ev.dataTransfer.files[0].type.includes("image")) {
        this.archivo = ev.dataTransfer.files[0].name;
        this.$refs.files.files = ev.dataTransfer.files;
        this.datos = true;
      } else {
        alert("coloque un archivo de tipo imagen");
      }
    },
    submit() {
      let data = new FormData(this.$refs.form);
      console.log(data.get("myfile"));

      axios({
        method: "post",
        url: "https://regalonavidad.herokuapp.com/file",
        data,
        headers: {
          "Content-Type": "multipart/form-data",
        },
      }).then((val) => {
        this.done = val.data.done;
      });
    },
    select() {
      this.archivo = this.$refs.files.files[0].name;
      this.datos = true;
    },
  },
};
</script>

<style>
body {
  background: white;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.file {
  height: 200px;
  margin: 0 auto;
  border: #2c3e50 solid 2px;
  color: rgb(52, 95, 133);
  margin-bottom: 1em;
}
.file-enter {
  background-color: rgb(113, 136, 170);
  color: white;
}

.container-file {
  position: relative;
}
.file > h4 {
  margin-top: 1.1em;
}
h4 {
  font-size: 1em;
}
.file-select {
  position: absolute;
  display: block;
  width: 40%;
  background-color: #60778d;
  left: 30%;
  top: 40%;
  margin-top: 2em;
  color: aliceblue;
}
.enlace {
  color: #2c3e50;
}
a {
  color: #2c3e50;
  font-size: 1.2em;
}
</style>

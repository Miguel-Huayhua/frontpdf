<template>
  <div class="container">
    <form class="row mb-5" ref="form" @submit.prevent="submit">
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
            name="apellidos"
            id="apellidos"
            type="text"
            placeholder="Introduzca sus apellidos"
          />
        </div>
      </div>

      <div class="col-12 container-file">
        <label class="file-select" for="file"> Seleccione archivos </label>
        <input
          @change="select"
          ref="files"
          type="file"
          id="file"
          name="myfile"
          style="display: none"
          accept="image/*"
        />
        <div
          ref="file"
          draggable="true"
          @dragenter="dragenter"
          @drop.prevent="ondrop"
          @dragleave="dragleave"
          class="file"
          @dragover.prevent
        >
          <h4 v-if="datos">{{ archivo }}</h4>
          <h4 v-else>
            Arrastre para elegir un archivo...
            <br />
            <br />
            <h4>ó</h4>
          </h4>
        </div>
      </div>
      <input class="btn btn-secondary" type="submit" value="Generar Carta " />
    </form>
      <a href="https://pdf-generatormike.herokuapp.com/download" v-if="done">
        Puedes descargarlo acá :)</a
      >
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
    click() {
      console.log(this.$refs.file);
    },
    submit() {
      let form = new FormData(this.$refs.form);
      axios({
        method: "post",
        url: "https://pdf-generatormike.herokuapp.com/file",
        data: form,
        headers: { "Content-Type": "multipart/form-data" },
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

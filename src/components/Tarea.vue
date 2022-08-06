<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col-sm-12 col-md-8 offset-md-2">
        <input
          class="form-control"
          type="text"
          placeholder="Ingrese una tarea"
          v-model="nombreTarea"
          @keyup.enter="agregarTarea"
        />
      </div>
      <div class="col-sm-12 col-md-6 offset-md-3 mt-2">
        <div v-if="tareas.length === 0" class="card p-2 alert alert-warning">
          <h6 class="text-center">No hay tareas para mostrar...</h6>
        </div>
        <!--Aqui vamos a renderizar nuestras tareas-->
        <ul class="list-group">
          <li
            v-for="(tarea, index) of tareas"
            :key="index"
            class="
              list-group-item
              d-flex
              align-items-center
              justify-content-between
            "
          >
            <span class="cursor" @click="actualizarTarea(tarea, index)">
              <i
                :class="
                  tarea.estado
                    ? 'fas fa-check-circle text-success'
                    : 'far fa-circle'
                "
              ></i>
              
            </span>
            <h5>{{ tarea.nombre }}</h5>
            <span class="cursor text-danger" @click="eliminarTarea(index)">
              <i class="fa fa-trash-alt"></i>
            </span>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tareas: [],
      nombreTarea: "",
    };
  },
  methods: {
    agregarTarea() {
      if (this.nombreTarea === "") {
        this.$swal.fire({
          title: "Debes crear una tarea!",
          showClass: {
            popup: "animate__animated animate__fadeInDown",
          },
          hideClass: {
            popup: "animate__animated animate__fadeOutUp",
          },
        });
        return;
      }
      // console.log('Agregando una tarea...');

      // Nuestra tarea
      const tareaObject = {
        nombre: this.nombreTarea,
        estado: false,
      };

      this.nombreTarea = "";
      // console.log(tareaObject);
      this.tareas.push(tareaObject);
      console.log(this.tareas);
      localStorage.setItem("tareas-storage", JSON.stringify(this.tareas));
    },
    eliminarTarea(indice) {
      this.$swal.fire({
          title: "¿Estás seguro?",
          text: "¡No podrás revertir esto!",
          icon: "warning",
          showCancelButton: true,
          confirmButtonColor: "#3085d6",
          cancelButtonColor: "#d33",
          confirmButtonText: "Sí, eliminar tarea!",
        })
        .then((result) => {
          if (result.isConfirmed) {
            // tareas es un arreglo, se cortaría según el índice
            // indice: indicamos de donde empezar a eliminar
            // 1: indicamos cuántos elementos deseamos eliminar
            this.tareas.splice(indice, 1);
            /*
              * En este punto tenemos un arreglo con las tareas eliminadas
              * Luego, actualizamos el localstorage con la tarea que se eliminó
            */
            localStorage.setItem("tareas-storage", JSON.stringify(this.tareas));
            Swal.fire("Eliminada!", "Tu tarea ha sido eliminada.", "success");
          }
        });
    },
    actualizarTarea(tarea, indice) {
      this.tareas[indice].estado = !tarea.estado; // this.tarea esta en false
      localStorage.setItem("tareas-storage", JSON.stringify(this.tareas));
      console.log(this.tareas);
    },
  },
  created() {
    // llamar info que existe dentro del local storage
    if (localStorage.getItem("tareas-storage")) {
      console.log("Existe info en el local storage");
      this.tareas = JSON.parse(localStorage.getItem("tareas-storage"));
    }
  },
};
</script>

<style>
.cursor {
  cursor: pointer;
}
</style>
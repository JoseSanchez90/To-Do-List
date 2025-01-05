<script setup>
    import Trash from './Icons/Trash.vue'
    import Edit from './Icons/Edit.vue'
    import { ref } from 'vue'

    // Lista de tareas con un estado de completado
    const tasks = ref([])
    const newTask = ref('')
    const editingIndex = ref(null)
    const editedTask = ref('')

    // Agregar una tarea
    const addTask = () => {
        if (newTask.value.trim() !== '') {
            tasks.value.push({ text: newTask.value, completed: false })
            newTask.value = ''
        }
    }

    // Eliminar una tarea
    const deleteTask = (index) => {
        tasks.value.splice(index, 1)
    }

    // Activar el modo edición
    const startEditing = (index) => {
        editingIndex.value = index
        editedTask.value = tasks.value[index].text
    }

    // Guardar la tarea editada
    const saveTask = (index) => {
        if (editedTask.value.trim() !== '') {
            tasks.value[index].text = editedTask.value
            editingIndex.value = null
            editedTask.value = ''
        }
    }

    // Cambiar el estado completado de la tarea
    const toggleTask = (index) => {
        tasks.value[index].completed = !tasks.value[index].completed
    }
</script>

<template>
    <div class="container">
        <div>
            <h1>To-Do List</h1>
        </div>
        <div class="sub-container">
            <div class="tittle">
                <p class="sub-tittle">Nueva tarea:</p>
            </div>
            <div class="form">
                <input type="text" v-model="newTask" class="write">
                <button type="submit" @click="addTask" class="btn add">Agregar</button>
            </div>
        </div>
        <div class="task-container">
            <!-- Mensaje de "No hay tareas" -->
            <p v-if="tasks.length === 0" class="no-tasks">- No hay tareas -</p>

            <!-- Lista de tareas -->
            <p v-for="(task, index) in tasks" :key="index" class="sub-list">
                <!-- Mostrar el campo de edición si estamos editando esta tarea -->
                <div v-if="editingIndex === index" class="edit-list">
                    <input type="text" v-model="editedTask" class="edit-task">
                    <button @click="saveTask(index)" class="btn save">Guardar</button>
                </div>
                <!-- Mostrar la tarea normal si no está en modo edición -->
                <div v-else class="task-options">
                    <label class="task-check">
                        <input type="checkbox" id="check" hidden="" class="task-checkbox" :checked="task.completed" @change="toggleTask(index)" />
                        <label for="check" class="checkmark"></label>
                        <span :class="{ 'completed-task': task.completed }">
                            {{ index + 1 }}. {{ task.text }}</span>
                    </label>
                    <div class="buttons-options">
                        <button v-if="!task.completed" @click="startEditing(index)" class="edit">
                            <Edit />
                        </button>
                        <button @click="deleteTask(index)" class="trash">
                            <Trash />
                        </button>
                    </div>
                </div>
            </p>
        </div>
    </div>
</template>

<style>

    * {
        padding: 0;
        margin: 0;
        font-family: "Outfit", serif;
        font-optical-sizing: auto;
        font-weight: 500;
        font-style: normal;
    }

    .container {
        height: 100vh; /* Ajusta exactamente a la altura de la pantalla */
        display: flex;
        flex-direction: column;
        align-items: center;
        padding: 100px; /* Espacio interno */
        box-sizing: border-box; /* Asegura que padding no aumente el tamaño total */
        gap: 30px;
        background-image: url('../assets/bg.jpg'); /* Imagen de fondo */
        background-size: cover; /* La imagen cubre todo el contenedor */
        background-repeat: no-repeat; /* Evita repetir la imagen */
        background-position: center; /* Centra la imagen en el contenedor */
    }

    .sub-container {
        width: 500px;
        height: 50px;
        padding: 18px;
        background-color: white;
        display: flex;
        flex-direction: column;
        border: 1px solid gray;
        box-shadow: 4px 4px 1px rgb(100, 135, 255);
        gap: 5px;
    }

    .new-task {
        font-size: 14px;
        font-weight: bold;
        color: black;
    }

    .form {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        align-items: center;
    }

    .write {
        width: 350px;
        height: 10px;
        padding: 8px;
        font-size: 14px;
    }

    h1 {
        font-size: 38px;
        font-weight: 800;
        color: white;
    }

    .btn {
        width: 100px;
        height: 30px;
        font-size: 14px;
        font-weight: 500;
        z-index: 2;
        border-radius: 4px; /* Bordes redondeados */
        border: none; /* Sin borde */
        background-color: rgb(0, 32, 142); /* Color de fondo */
        color: #ffffff; /* Color del texto */
        position: relative; /* Posicionamiento relativo para manejar la sombra */
        box-shadow: 3px 5px 0px rgb(0, 58, 255); /* Sombra inicial */
        cursor: pointer; /* Cambia el cursor a pointer */
        transition: transform 0.1s ease, box-shadow 0.1s ease;
    }

    .btn:hover {
        background-color: rgb(0, 23, 100); /* Cambia el color al pasar el ratón */
  }
  
  .btn:active {
        transform: translate(4px, 6px); /* Mueve el botón hacia abajo y a la derecha */
        box-shadow: 0 0 0 rgb(0, 58, 255); /* Mantén la sombra fija al presionar */
        background-color: rgb(0, 32, 142); /* Cambia el color al presionar */
  }

    .task-container {
        width: 500px;
        overflow-y: auto; 
        padding: 18px;
        background-color: white;
        display: flex;
        flex-direction: column;
        justify-content: flex-start;
        border: 1px solid gray;
        box-shadow: 4px 4px 1px rgb(100, 135, 255);
    }

    .task-options {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        align-items: center;
        width: 100%;  /* Asegura que ocupe el 100% de su contenedor */
        padding: 10px 0px;
        border-bottom: 1px solid gray;
    }

    .task-check {
        display: flex;
        flex-direction: row;
        justify-content: start;
        align-items: center;
        gap: 10px;
        width: 100%;
        word-wrap: break-word; 
    }

    .task-checkbox {
        font-size: 16px;
    }

    .buttons-options {
        display: flex;
        flex-direction: row;
        gap: 10px;
    }

    .no-tasks {
        color: gray;
        font-size: 16px;
        text-align: center;
    }


    .sub-list {
        list-style: none;
    }

    .edit-list {
        border-style: none;
        cursor: pointer;
        background-color: transparent;
        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: space-between;
    }

    .trash, .edit {
        border-style: none;
        cursor: pointer;
        background-color: transparent;
        transition: transform 0.3s ease; /* Transición suave */
    }

    .trash:hover, .edit:hover {
        transform: scale(1.2); /* Aumenta el tamaño del botón en un 20% */
    }

    .edit-task {
        width: 350px;
        height: 10px;
        padding: 8px;
        font-size: 14px;
    }

    .task-write {
        font-weight: bold;
        color: black;
    }

    .task-check span {
        color: black;
        display: inline-block;  /* Permite que el texto se ajuste dentro del contenedor */
        width: 350px;  /* Asegura que no se desborde el contenedor */
        line-height: 1.4;
    }

    .task-check span.completed-task {
        color: gray; /* Cambia el texto a gris */
        text-decoration: line-through; /* Tacha el texto */
        font-size: 16px;
    }

    .checkmark {
        display: block;
        width: 20px;
        height: 20px;
        background-color: rgb(126, 156, 255);
        border-radius: 4px;
        position: relative;
        transition: background-color 0.4s;
        overflow: hidden;
        cursor: pointer;
    }

    #check:checked ~ .checkmark {
        background-color: #08bb68;
    }

    .checkmark::after {
        content: "";
        position: absolute;
        width: 5px;
        height: 10px;
        border-right: 3px solid #fff;
        border-bottom: 3px solid #fff;
        top: 44%;
        left: 50%;
        transform: translate(-50%, -50%) rotateZ(40deg) scale(10);
        opacity: 0;
        transition: all 0.4s;
    }

    #check:checked ~ .checkmark::after {
        opacity: 1;
        transform: translate(-50%, -50%) rotateZ(40deg) scale(1);
    }

/* Para móviles y pantallas pequeñas (480px o menos) */
@media (max-width: 580px) {
    .sub-container {
        width: 320px; /* Reduce aún más el ancho */
        padding: 16px; /* Reduce el padding */
        gap: 10px;
    }

    .task-container {
        width: 320px; /* Reduce aún más el ancho */
        max-height: 300px;
        padding: 16px; /* Reduce el padding */
        gap: 10px;
        overflow-y: auto; 
        padding: 10px;
        justify-content: flex-start;
    }

    .sub-tittle {
        font-size: 12px;
        font-weight: 400;
    }

    .write {
        width: 200px;
    }

    h1 {
        font-size: 24px;
        font-weight: 600;
        color: white;
    }

    .btn {
        width: 70px;
        height: 30px;
        font-size: 12px;
        text-decoration: none;
        font-weight: 500;
    }

    .task-options {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        align-items: center;
        width: 100%;  /* Asegura que ocupe el 100% de su contenedor */
        border-bottom: 1px solid gray;
    }

    .edit-task {
        width: 200px;
        height: 10px;
        padding: 8px;
        font-size: 12px;
    }

    .buttons-options {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        gap: 10px;
    }

    .checkmark {
        display: block;
        width: 20px;
        height: 20px;
        background-color: rgb(126, 156, 255);
        border-radius: 4px;
        position: relative;
        transition: background-color 0.4s;
        overflow: hidden;
        cursor: pointer;
    }

    #check:checked ~ .checkmark {
        background-color: #08bb68;
    }

    .checkmark::after {
        content: "";
        position: absolute;
        width: 5px;
        height: 10px;
        border-right: 3px solid #fff;
        border-bottom: 3px solid #fff;
        top: 44%;
        left: 50%;
        transform: translate(-50%, -50%) rotateZ(40deg) scale(10);
        opacity: 0;
        transition: all 0.4s;
    }

    #check:checked ~ .checkmark::after {
        opacity: 1;
        transform: translate(-50%, -50%) rotateZ(40deg) scale(1);
    }

    .task-check span {
        color: black;
        display: inline-block;  /* Permite que el texto se ajuste dentro del contenedor */
        width: 150px;  /* Asegura que no se desborde el contenedor */
        line-height: 1.4;
        font-size: 12px;
    }

}

@media (max-width: 360px) {

    .container {
        height: 100vh; /* Ajusta exactamente a la altura de la pantalla */
        display: flex;
        flex-direction: column;
        align-items: center;
        padding: 20px; /* Espacio interno */
        box-sizing: border-box; /* Asegura que padding no aumente el tamaño total */
        gap: 20px;
        background-image: url('../assets/bg.jpg'); /* Imagen de fondo */
        background-size: cover; /* La imagen cubre todo el contenedor */
        background-repeat: no-repeat; /* Evita repetir la imagen */
        background-position: center; /* Centra la imagen en el contenedor */
    }
    .sub-container {
        width: 270px; /* Reduce aún más el ancho */
        padding: 15px; /* Reduce el padding */
        gap: 10px;
    }

    .task-container {
        width: 270px; /* Reduce aún más el ancho */
        max-height: 300px;
        padding: 15px; /* Reduce el padding */
        gap: 10px;
        overflow-y: auto; 
        justify-content: flex-start;
    }

    .sub-tittle {
        font-size: 12px;
        font-weight: 400;
    }

    .write {
        width: 170px;
    }

    h1 {
        font-size: 22px;
        font-weight: 600;
        color: white;
    }

    .btn {
        width: 70px;
        height: 30px;
        font-size: 12px;
        text-decoration: none;
        font-weight: 500;
    }

    .task-options {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        align-items: center;
        width: 100%;  /* Asegura que ocupe el 100% de su contenedor */
        border-bottom: 1px solid gray;
    }

    .edit-task {
        width: 170px;
        height: 10px;
        padding: 8px;
        font-size: 12px;
    }

    .buttons-options {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        gap: 10px;
    }

    .checkmark {
        display: block;
        width: 10px;
        height: 10px;
        background-color: #ddd;
        border: 2px solid rgb(50, 50, 50);
        border-radius: 50%;
        position: relative;
        transition: background-color 0.4s;
        overflow: hidden;
        cursor: pointer;
    }

    #check:checked ~ .checkmark {
        background-color: #08bb68;
    }

    .checkmark::after {
        content: "";
        position: absolute;
        width: 5px;
        height: 10px;
        border-right: 3px solid #fff;
        border-bottom: 3px solid #fff;
        top: 44%;
        left: 50%;
        transform: translate(-50%, -50%) rotateZ(40deg) scale(10);
        opacity: 0;
        transition: all 0.4s;
    }

    #check:checked ~ .checkmark::after {
        opacity: 1;
        transform: translate(-50%, -50%) rotateZ(40deg) scale(1);
    }

    .task-check span {
        color: black;
        display: inline-block;  /* Permite que el texto se ajuste dentro del contenedor */
        width: 150px;  /* Asegura que no se desborde el contenedor */
        line-height: 1.4;
        font-size: 12px;
    }
}

</style>

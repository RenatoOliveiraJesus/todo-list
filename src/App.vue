<template>
  <main id="app">
    <section class="todo-list">
      <h3>Lista de Tarefas</h3>
      <div v-for="tarefa in tarefa" :key="tarefa.id" class="all-todos">
        <div class="single-todo" :class="{ done: tarefa.active }">
          <p
            style="text-align: left; width: 70%"
            @click="tarefa.active = !tarefa.active,StoreTarefa()"
          >
            {{ tarefa.descricao }}
          </p>
          <p style="text-align: right; width: 30%">
            <button class="clearid" @click="DeleteTarefa(tarefa.id),StoreTarefa()">
              Apagar
            </button>
          </p>
        </div>
      </div>
      <button class="clear" @click="RemoverTodasTarefas(),StoreTarefa()">Limpar tudo</button>
      <input
        type="text"
        placeholder="Escreva uma nova tarefa..."
        v-model="newTarefa.descricao"
      />
      <button class="add" :disabled="!newTarefa.descricao" @click="addTarefa(),StoreTarefa()">
        Adicionar
      </button>
      <div class="instructions">
        Instruções:
        <ul>
          <li>
            Clique no texto da tarefa para alternar entre feita / não feita
          </li>
          <li>Use o botão limpar tudo para remover todas as tarefas</li>
          <li>Use o input para adicionar novas tarefas</li>
        </ul>
      </div>
    </section>
  </main>
</template>



<script>
export default {
  name: "app",
  data() {
    return {
      tarefa: [],
      newTarefa: {},
    };
  },
  methods: {
    addTarefa: function () {
      if (this.tarefa) {
        this.tarefa.push({
          id: Date.now(),
          descricao: this.newTarefa.descricao,
          active: false,
        });

        localStorage.setItem("Tarefas", JSON.stringify(this.tarefa));

        this.newTarefa = {};
      } else {
        this.tarefa = [];
      }
    },
    StoreTarefa() {
      localStorage.setItem("Tarefas", JSON.stringify(this.tarefa));
      console.log("updated");
    },

    DeleteTarefa: function (id) {
      this.tarefa = this.tarefa.filter((i) => i.id != id);
    },

    RemoverTodasTarefas: function () {
      this.tarefa = [];
    },
  },
  created() {
    this.tarefa = localStorage.getItem("Tarefas")
      ? JSON.parse(localStorage.getItem("Tarefas"))
      : this.tarefa;
  },

};
</script>


<style>
body {
  margin: 0;
  font-family: "Open Sans", sans-serif;
}

main {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  flex-wrap: wrap;
  padding-top: 60px;
}

main .todo-list input {
  box-sizing: border-box;
  height: 28px;
  border-radius: 0.25rem;
  width: 60%;
  border: 1px solid lightgrey;
  margin-top: 32px;
}

main button {
  background-color: transparent;
  cursor: pointer;
  box-sizing: border-box;
  height: 28px;
  border-radius: 0.25rem;
  color: #fff;
}

main button:hover {
  opacity: 0.8;
}

main button.add:disabled {
  background-color: rgb(163, 163, 163);
  border: 1px solid rgb(163, 163, 163);
}

main button.add:disabled:hover {
  background-color: rgba(163, 163, 163);
}

main button.add {
  background-color: #007bff;
  border: 1px solid #007bff;
  margin-left: 2px;
}

main button.clear {
  background-color: #dc3545;
  border: 1px solid #dc3545;
  display: block;
  margin: auto;
}

.clearid {
  background-color: #e80606;
  color: whitesmoke;
  border: 2px solid;
  border-radius: 5px;
}

main button:focus {
  outline: 0;
}

main > section.todo-list h3 {
  text-align: center;
  margin-top: 24px;
  width: 100%;
}

main > section.todo-list {
  flex-wrap: wrap;
  border: 1px solid lightgrey;
  background-color: rgb(248, 248, 248);
  padding: 20px;
  max-width: 500px;
  min-width: 300px;
  text-align: center;
  border-radius: 0.25rem;
}

main .all-todos .single-todo {
  display: flex;
  align-items: center;
  justify-content: center;
}

main .all-todos .single-todo p {
  margin: 12px 0;
  cursor: pointer;
}

main .all-todos .single-todo.done p {
  text-decoration: line-through;
  color: blue;
}

main .all-todos .single-todo button.remove {
  width: 12px;
  height: 12px;
  border: none;
  /* background: transparent url('img/remove.png') no-repeat center; */
  background-size: contain;
  cursor: pointer;
  margin-left: 8px;
}

main > section.todo-list button.clear {
  margin-top: 24px;
}

div.instructions {
  font-size: 11px;
  margin-top: 40px;
  color: grey;
}

div.instructions ul {
  list-style: inside;
  text-align: center;
  padding: 0;
}

div.instructions ul li {
  margin: 4px auto;
}
</style>

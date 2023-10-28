<script>
export default {
  data() {
    return {
      // itens a ser posto iniciamente na tabela
      items: [
        { Nome: 'Alice', Email: 'alice@example.com', Telefone: '123-456-7890' },
        { Nome: 'Bob', Email: 'bob@example.com', Telefone: '987-654-3210' },
      ],
      // recebimento de nova itens numa nova lista
      novoItem: {
        Nome: '',
        Email: '',
        Telefone: '',
      },
      // comando para abrir layouts de adicao e edicao
      showOverlay: false,
      editMode: false,
      editIndex: -1,
    };
  },
  methods: {
    // metodo de editar itens da listas
    editarItem(index) {
      this.editMode = true;
      this.editIndex = index;
      this.novoItem = { ...this.items[index] };
      this.showOverlay = true;
    },
    // fechamento de overlay apos a adicao de uma nova lista encapsulando os itens 
    fecharOverlay() {
      this.editMode = false;
      this.editIndex = -1;
      this.novoItem = {
        Nome: '',
        Email: '',
        Telefone: '',
      };
      this.showOverlay = false;
    },
    //abrir overlay de edicao e fechamento do overlay
    adicionarDado(event) {
      event.preventDefault();

      if (this.editMode) {
        this.items[this.editIndex] = { ...this.novoItem };
      } else {
        this.items.push({ ...this.novoItem });
      }

      this.fecharOverlay();
    },
    // excluir apenas uma lista da tabela
    excluirItem(index) {
      this.items.splice(index, 1);
    },
  },
};
</script>

<template>
  <div>
    <table>
      <thead>
        <tr>
          <!--header da tabela-->
          <th>Nome</th>
          <th>Email</th>
          <th>Telefone</th>
          <th class="plus">
            <!--botão de abertura do overlay de adicionar mais uma lista-->
            <button @click="showOverlay = true">
              <svg xmlns="http://www.w3.org/2000/svg" width="21" height="21" viewBox="0 0 21 21" fill="none">
                <rect x="8.39999" width="4.2" height="21" fill="#010101"/>
                <rect y="8.39999" width="21" height="4.62" fill="#010101"/>
              </svg>
            </button>
          </th>
        </tr>
      </thead>
      <tbody>
        <!--renderizacao de cade lista com seus itens-->
        <tr v-for="(item, index) in items" :key="item.Nome">
          <td class="nome" :title="item.Nome">{{ item.Nome }}</td>
          <td class="email" :title="item.Email">{{ item.Email }}</td>
          <td class="telefone" :title="item.Telefone">{{ item.Telefone }}</td>
          <td class="modif">
            <!--botão para abrir o overlay de editar a lista selecionada-->
            <button @click="editarItem(index)">
              <svg xmlns="http://www.w3.org/2000/svg" width="2" height="11" viewBox="0 0 2 11" fill="none">
                <ellipse cx="1" cy="1.03251" rx="1" ry="1.03251" fill="black"/>
                <ellipse cx="1" cy="5.16254" rx="1" ry="1.03251" fill="black"/>
                <ellipse cx="1" cy="9.29257" rx="1" ry="1.03251" fill="black"/>
              </svg>
            </button>
            <button @click="excluirItem(index)">Excluir</button>
          </td>
          <hr>
        </tr>
      </tbody>
    </table>
  </div>
  <div v-if="showOverlay" class="overlay">
    <!--iniciador do overlay de adicionar e editar, ambos sendo decidido pela logica do js a cima-->
    <form @submit="adicionarDado">
      <!--botão de fechamento do overlay-->
      <button class="close" @click="fecharOverlay" type="button">
        <svg xmlns="http://www.w3.org/2000/svg" width="21" height="21" viewBox="0 0 21 21" fill="none">
            <rect x="8.39999" width="4.2" height="21" fill="#010101"/>
          <rect y="8.39999" width="21" height="4.62" fill="#010101"/>
        </svg>
      </button>
      <h1>{{ editMode ? 'Editar' : 'Adicionar' }}</h1>
      <input v-model="novoItem.Nome" placeholder="Nome">
      <input v-model="novoItem.Email" placeholder="Email">
      <input v-model="novoItem.Telefone" placeholder="Telefone">
      <!--verificador de qual tipo de overlay vai ser aberto-->
      <button type="submit">{{ editMode ? 'Salvar' : 'Adicionar' }}</button>
    </form>
  </div>
</template>

<style scoped>
div{
  width: fit-content;
  background-color: #000;
}
table{
  display: flex;
  flex-direction: column;
}
thead{
  padding: 2em 2em;
  border: 1em solid #000;
  background-color: #2196F3;
}
thead tr{
  display: flex;
  gap: 12em;
}
button{
  background-color: transparent;
  border: none;
  cursor: pointer;
  width: 2.4em;
}
.nome,
.email{
  width: 12.4em;
  overflow: hidden;
}
.telefone{
  max-width: 125px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.modif{
  width:9.2em;
  text-align: end;
}
th{
  font-size: 1.4em;
  color: #fff;
  font-weight: 900;
  line-height: normal;
}
tbody{
  display: flex;
  flex-direction: column;
  gap: 1em;
  height: 35em;
  overflow: hidden;
  overflow-y: auto;
  padding: 0 2em;
  border: 1em solid #000;
  background-color: #97d0ff;
}
::-webkit-scrollbar{
  display: none;
}
tbody tr:nth-child(1){
  padding: 1em 0 0 0;
}
td{
  color: #000;
  font-size: 1.3em;
  font-weight: 600;
}
hr{
  margin-top: 1em;
}
/* fundo opaco do overlay e de posicionamento do mesmo*/
.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

/* estilos para o formulário e botões dentro do overlay */
.overlay form {
  justify-self: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 40em;
  gap: 2em;
  background-color: #66dcfd;
  padding: 2em;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
}

.overlay button {
  font-size: 0.8em;
  font-weight: 600;
  margin: 2em;
  padding: 1em 2em;
  border-radius: 0.5em;
  width: fit-content;
  background-color: #2196F3;
}
h1{
  color: #000;
  font-weight: 700;
}
input{
  width: 30em;
  height: 3em;
  border: solid #000;
  border-radius: 0.3em;
}
button[class="close"]{
  margin: 0 0 0 45em;
  top: 11em;
  position: absolute;
  background-color: transparent;
  rotate: 45deg;
}
input:focus{
  outline: none;
}
</style>

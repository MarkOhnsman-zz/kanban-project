<template>
  <div class="boards container-fluid">
    <div class="row" id="boards-nav">
      <div class="col">
        WELCOME TO THE BOARDS!!!
      </div>
      <div class="col">
        <form @submit.prevent="addBoard">
          <input type="text" placeholder="title" v-model="newBoard.title" required>
          <input type="text" placeholder="description" v-model="newBoard.description">
          <button type="submit">Create Board</button>
        </form>

      </div>
    </div>
    <div class="row">
      <div class="col">
        <div v-for="board in boards" :key="board._id">
          <div class="card" style="width: 18rem;">
            <div class="card-body">
              <router-link :to="{name: 'board', params: {boardId: board._id}}">
                <h5 class="card-title">{{board.title}}</h5>
              </router-link>
              <p class="card-text">{{board.description}}</p>
              <button @click="deleteBoard(board._id)">DELETE BOARD</button>

            </div>
          </div>
        </div>

      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: "boards",
    created() {
      //blocks users not logged in
      if (!this.$store.state.user._id) {
        this.$router.push({ name: "login" });
      }
    },
    mounted() {
      this.$store.dispatch("getBoards");
    },
    data() {
      return {
        newBoard: {
          title: "",
          description: ""
        }
      };
    },
    computed: {
      boards() {
        return this.$store.state.boards;
      }
    },
    methods: {
      addBoard() {
        this.$store.dispatch("addBoard", this.newBoard);
        this.newBoard = { title: "", description: "" };
      },
      deleteBoard(boardId) {
        this.$store.dispatch("deleteBoard", boardId);
      }
    }
  };
</script>
<style>
  #boards-nav {
    height: 80px;
    background-color: rgba(10, 10, 10, .3);
    z-index: 1;
  }

  .boards {
    background-image: url(https://images.unsplash.com/photo-1496450681664-3df85efbd29f?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1050&q=80);
    min-height: 100vh;
    background-attachment: fixed;
  }
</style>
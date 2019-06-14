<template>
  <div class="boards">
    <div class="container-fluid">
      <div class="row" id="boards-nav">
        <div class="col" id="title" style="font-size: 3rem">
          Fidget
        </div>
        <div class="col" id="quote" style="font-size: 1.3rem; font-family: 'Comfortaa'">
          <div class="d-flex justify-content-center" id="tagline">
            Create some clarity!

          </div>
        </div>
        <div class="col">
          <form @submit.prevent="addBoard">
            <input type="text" placeholder="title" v-model="newBoard.title" required>
            <input type="text" placeholder="description" v-model="newBoard.description">
            <button type="submit">Create Board</button>
          </form>

        </div>
      </div>
    </div>

    <div class="container" id="board-container">
      <div class="row">
        <div class="col" v-for="board in boards" :key="board._id">
          <div class="card" style="width: 18rem;">
            <div class="card-body">
              <router-link :to="{name: 'board', params: {boardId: board._id}}">
                <h5 class="card-title board-name">{{board.title}}</h5>

              </router-link>
              <p class="card-text">{{board.description}}</p>

              <button class="btn btn-outline-secondary" style="color: whitesmoke"
                @click.stop="deleteBoard(board._id)"><i class="fas fa-trash-alt"></i></button>

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
        timeout: 3000;
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
    background-position: center;

  }

  #tagline {
    position: absolute;
    bottom: 0;
  }

  .board-name {
    font-family: 'Pacifico', cursive;
    font-size: 1.5rem;
    color: whitesmoke;
  }

  .boards {
    background-image: url(https://images.unsplash.com/photo-1470770903676-69b98201ea1c?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1950&q=80);
    background-color: rgb(92, 95, 94);
    min-height: 100vh;
    background-attachment: fixed;
    background-size: contain;
    background-size: cover;
    background-repeat: no-repeat;
  }
</style>
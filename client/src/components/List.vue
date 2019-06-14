<template>
  <div class="list">
    <div class="accordion" id="accordionExample">
      <div class="card">
        <div class="card-header" id="headingOne">
          <h2 class="mb-0">
            <button class="btn btn-link" type="button" data-toggle="collapse" data-target="#collapseOne"
              aria-expanded="true" aria-controls="collapseOne">
              {{listData.title}} </button>
          </h2>
        </div>

        <div id="collapseOne" class="collapse show" aria-labelledby="headingOne" data-parent="#accordionExample">
          <div class="card-body">
            <!-- Task Description goes Here -->
          </div>
        </div>
      </div>
      <div class="card">
        <div class="card-header" id="headingTwo">
          <h2 class="mb-0">
            <button class="btn btn-link collapsed" type="button" data-toggle="collapse" data-target="#collapseTwo"
              aria-expanded="false" aria-controls="collapseTwo">
              Collapsible Group Item #2
            </button>
          </h2>
        </div>
        <div id="collapseTwo" class="collapse" aria-labelledby="headingTwo" data-parent="#accordionExample">
          <div class="card-body">
            <!-- Add A Task goes Here -->
            <task v-for="task in tasks" :taskData="task" />
            <p>
              <button class="btn btn-primary" type="button" data-toggle="collapse" data-target="#collapseExample"
                aria-expanded="false" aria-controls="collapseExample">
                <i class="fas fa-plus"></i>
              </button>
            </p>
            <div class="collapse" id="collapseExample">
              <div class="card">
                <form class="form-inline" @submit.prevent="submitTask">
                  <div class="form-group mx-sm-3 mb-2">
                    <input type="text" class="form-control" placeholder="Title" v-model='newTask.title'>
                  </div>
                  <div class="form-group mx-sm-3 mb-2">
                    <input type="text" class="form-control" placeholder="Description" v-model='newTask.description'>
                  </div>
                  <button type="submit" class="btn btn-primary mb-2">Add Task</button>
                </form>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="card">
        <div class="card-header" id="headingThree">
          <h2 class="mb-0">
            <button class="btn btn-link collapsed" type="button" data-toggle="collapse" data-target="#collapseThree"
              aria-expanded="false" aria-controls="collapseThree">
              Collapsible Group Item #3
            </button>
          </h2>
        </div>
        <div id="collapseThree" class="collapse" aria-labelledby="headingThree" data-parent="#accordionExample">
          <div class="card-body">
            <!-- Maybe something will go here -->
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import Task from '@/components/Task.vue'
  export default {
    name: 'List',
    props: ['listData'],
    data() {
      return {
        newTask: {
          title: '',
          description: '',
          listId: this.listData._id
        }
      }
    },
    mounted() {
      this.$store.dispatch('getTasks', this.listData._id)
    },
    computed: {
      tasks() {
        return this.$store.state.tasks[this.listData._id]
      }
    },
    methods: {
      deleteList() {
        this.$store.dispatch('deleteList', this.listData);
      },
      submitTask({ target: form }) {
        debugger
        this.$store.dispatch('addTask', this.newTask)
        form.reset()
      },

    },
    components: {
      Task
    }

  }
</script>

<style scoped>
</style>
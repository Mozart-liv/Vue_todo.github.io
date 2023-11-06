<template>
    <div>
        <h2 class="text-danger text-center bg-dark p-2">{{ title }}'s To do </h2>

        <div class="container">
            <div class="row my-3">
                <div class="col-8">
                    <input type="text" v-model="newTask" @change="addTask()" class="form-control" id="">
                </div>
                <div class="col">
                    <input type="button"  @click="addTask()" class="btn btn-secondary" value="Add">
                </div>
                <div class="col">
                    <input type="button"  @click="deleteTask()" class="btn btn-danger" value="Delete Task">
                </div>
            </div>
            <div class="row m-auto mb-3">
                <div class="col">Tasks</div>
                <div class="col-2">Done</div>
            </div>

            <div class="" v-if="filterTask.length > 0">
                <div class="row m-auto mt-3" v-for="(task, index) in filterTask" :key="index">
                    <div class="col" :class="task.done ? 'done' : ''">{{ task.action }}</div>
                    <div class="col-2">
                        <input type="checkbox" v-model="task.done" >
                    </div>
                </div>
            </div>
            <div v-else>
                <div class="alert alert-warning">There is no task yet</div>
            </div>

            <div class="row m-auto bg-danger text-white mt-3 p-1">
                <span class="col">Hide complete task</span>
                <input type="checkbox" class="col-2" v-model="hide">
            </div>
        </div>
    </div>
</template>

<script>
export default{
    name : "App",
    data : ()=>({
        title : "mozart",
        hide : false,
        newTask : "",
        tasks : []
    }),
    computed: {
        filterTask() {
            return this.hide ? this.tasks.filter((v) => !v.done) : this.tasks;
        },
    },
    methods: {
        addTask(){
            if(this.newTask == ""){
               return alert("type task");
            }

            this.tasks.push({
                action: this.newTask,
                done: false
            });

            this.storeData();

            this.newTask = "";
        },

        deleteTask(){
            this.tasks = this.tasks.filter((v) => !v.done);
            this.storeData();
        },

        storeData(){
            localStorage.setItem("taskData", JSON.stringify(this.tasks));
        }
    },

    mounted(){
        let data = localStorage.getItem("taskData");
        if(data !== null){
            this.tasks = JSON.parse(data);
        }
    }
};
</script>

<style>
 .done {
    text-decoration: line-through;
 }
</style>

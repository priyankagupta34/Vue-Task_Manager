<template>
    <div>
        <MainHeader />

        <div class="individual subtitle">
            <div class="flexSpaceBetween">
                <div>
                    <small class="subtitle"><b>Status:  </b>{{completeTaskInfo.status}}</small><br/>
                    <small class="subtitle"><b>Task ID:  </b>{{completeTaskInfo.taskid}}</small><br />
                    <small class="subtitle"><b>Name:  </b>{{completeTaskInfo.name}}</small>
                </div>
                <div>
                     <v-btn text icon style="color: lightblue " title="Go Back To Dashboard" v-on:click="goBackToDashboard()">
                        <v-icon>mdi-arrow-left-circle</v-icon>
                    </v-btn>

                    <v-btn text icon style="color: lightsalmon " title="Set Reminder" v-on:click="setReminder()">
                        <v-icon>mdi-alarm</v-icon>
                    </v-btn>
                </div>
            </div>
            <br />
            <br />
            <small class="subtitle"><b>*Addition of subtask is restricted to 7 per task. As idea behind creating task is to keep it simple and small. Create reminders for your tasks.</b></small>

            
            <div class="flexTing">
                <div class="sectionA">

                        <v-text-field style="width: 97%"
                            v-model="subtask"
                            v-on:change="subtaskCreation(subtask)"
                            autofocus
                            ref="subtask"
                            >
                        </v-text-field>

                    <div>
                        <textarea class="taskText"
                            v-model="description"
                            v-on:change="subtaskDescription(description)"
                            spellcheck="false"></textarea>
                    </div> 

                </div>
                
                <div class="sectionB">

                    <div>
                        <ul>
                            <transition-group enter-active-class="animated rotateIn" leave-active-class="animated rotateOut" >
                                <div  v-for="(task, index) in subtaskList" v-bind:key="task.subtaskid"  v-bind:index="index">

                                        <div v-if="task.subtaskid === presentSubtaskId" class="selectedSubTask">
                                            <li class="listSpecific" v-on:click="selectSubTask(index)">
                                                <div class="flexSpaceBetween">
                                                    <div>{{task.subtaskname}}</div>    
                                                    <div class="removeBtn" title="Remove Subtask" v-on:click.stop="removeSubTask(index)">
                                                        &#10005;
                                                    </div>
                                                </div>   
                                            </li>
                                            <div class="addBox" v-on:click="createNewSubtask(index)" >+</div>
                                        </div>

                                        <div v-if="task.subtaskid !== presentSubtaskId">
                                            <li class="listSpecific" v-on:click="selectSubTask(index)"
                                                >
                                                <div class="flexSpaceBetween">
                                                    <div>{{task.subtaskname}}</div>    
                                                    <div class="removeBtn" title="Remove Subtask" v-on:click.stop="removeSubTask(index)">
                                                        &#10005;
                                                    </div>
                                                </div> 
                                            </li>
                                            <div class="addBox" v-on:click="createNewSubtask(index)" >+</div>
                                        </div>

                                </div>
                                
                            </transition-group>
                        </ul>
                </div>
            </div>
        </div>

    </div>
    </div>
</template>

<script>

import MainHeader from './../main-header/MainHeader';
export default {

    components: {
        MainHeader
    },
    data(){
        return{
            completeTaskInfo: this.$route.params.completeTaskInfo,
            subtask:'',
            subtaskList:[],
            presentSubtaskId:'',
            description: ''
        }
    },
    methods: {

        subtaskCreation: function(subtask){
            this.subtaskList.forEach(item => {
                if(item.subtaskid === this.presentSubtaskId){
                    item.subtaskname = subtask;
                }
            })
        },
        subtaskDescription: function(description){
            this.subtaskList.forEach(item => {
                if(item.subtaskid === this.presentSubtaskId){
                    item.description = description;
                }
            })
        },
        createNewSubtask: function(index){
            if(this.subtaskList.length !== 7){
                let date = new Date();
                this.presentSubtaskId = date.getTime();
                this.subtaskList.splice(index+1,0,{subtaskid: this.presentSubtaskId, subtaskname: 'Untitled Page', description: ''});
                this.subtask = '';
                this.description = '';
                this.$refs.subtask.focus()
            }
        },
        
        selectSubTask: function(index){
            
            let task = this.subtaskList[index];
            this.presentSubtaskId = task.subtaskid;
            this.subtask = task.subtaskname;
            this.description = task.description;

        },
        removeSubTask: function(index){

            let task = this.subtaskList[index];
            if((this.subtaskList.length - 1) !== 0){
                if(this.presentSubtaskId === task.subtaskid){
                    if((index-1) !== -1){
                        let newTask = this.subtaskList[index-1];
                        this.presentSubtaskId = newTask.subtaskid;
                        this.subtask = newTask.subtaskname;
                        this.description = newTask.description;
                        this.subtaskList.splice(index, 1);
                    }else{
                        let newTask = this.subtaskList[index+1];
                        this.presentSubtaskId = newTask.subtaskid;
                        this.subtask = newTask.subtaskname;
                        this.description = newTask.description;
                        this.subtaskList.splice(index, 1);
                    }
                }else{
                    this.subtaskList.splice(index, 1);
                }
            }
        },
        goBackToDashboard: function(){
            this.$router.push({name: 'taskdashboard'});
        }
    },
    created: function() {
        
        let date = new Date();
        this.presentSubtaskId = date.getTime();
        if(this.completeTaskInfo.subtaskList.length === 0){
            this.subtaskList.push({subtaskid: this.presentSubtaskId, subtaskname: 'Untitled Page', description: ''});
            
        }
    },
    watch: {
        subtask: function(newSub, oldSub){
            if(newSub.trim() == ''){
                this.subtaskCreation('Untitled Page');
            }else{
                this.subtaskCreation(newSub);
            }
        },
        description: function(newSub, oldSub){
            this.subtaskDescription(newSub);
        }
    }
}
</script>

<style scoped src="./IndividualTask.css">

</style>
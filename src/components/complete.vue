<template>
  <div id="note">
    <div class="container">
      <form>
        <div class="form-group">
          <div class="d-flex w-100 justify-content-between"> 
            <label for="exampleInputEmail1">What do you want to remember?</label>
            
          </div>
          <div class="d-flex w-100 justify-content-between"> 
            <input type="text" v-model="noteName" class="form-control" id="Insert note field"  placeholder="Enter note name" v-on:keyup="addNote">
            <button v-else type="button" class="btn btn-secondary btn-sm" v-on:click="addNoteButton">Add</button>
          </div>
          <label for="exampleInputEmail1">Searching for a note?</label>
          <input class="form-control mr-sm-2" type="text" placeholder="Search" v-model="search" >
          <small id="emailHelp" class="form-text text-muted">Enter a short tittle for your note.</small>
          <hr />
          <div class="form-inline">
            <a class="text-muted"> {{totNotes}} pending tasks of a total of {{totCompleted}}  |</a>
            <a class="text-warning" v-on:click="deleteCompleted()" >| X Delete completed tasks.</a>
          </div>
        </div>
      </form>
      <div v-if="this.wordSearch != ''" v-bind:key="1">
        <transition-group name="custom-classes-transition" enter-active-class="animated bounceInLeft" leave-active-class="animated bounceOutRight">
          <div class="list-group" v-for="item in wordSearch" v-bind:key="item">
          <a href="#" class="list-group-item list-group-item-action flex-column align-items-start active">
            <div class="d-flex w-100 justify-content-between">
              <h5 v-if="item.state==false" >
                <img src="../assets/off.svg" style="width:'30px'" v-on:click="changeToComplete(item)"> {{item.noteName}}
              </h5>
              <h5 v-else >
                <img src="../assets/on.svg" style="width:'30px'" v-on:click="changeToComplete(item)"> {{item.noteName}}
              </h5>
              <small>{{item.date}}</small> 
            </div>
            <div class="d-flex w-100 justify-content-between"> 
              <div>
                <button v-if="item.priority == 'Low'" type="button" class="btn btn-info btn-sm" >Low</button>
                <button v-else type="button" class="btn btn-secondary btn-sm" v-on:click="changeToLow(item)">Low</button>
                <button v-if="item.priority == 'Normal'" type="button" class="btn btn-warning btn-sm">Normal</button>
                <button v-else type="button" class="btn btn-secondary btn-sm" v-on:click="changeToMedium(item)" >Normal</button>
                <button v-if="item.priority == 'High'" type="button" class="btn btn-danger btn-sm">High</button>
                <button v-else type="button" class="btn btn-secondary btn-sm" v-on:click="changeToHigh(item)" >High</button>
              </div>
              <div>
                <button  type="button" class="btn btn-success btn-sm" v-on:click="deleteNote(item)">Delete</button>
              </div>
            </div>
          </a>
        </div>
      </transition-group>
    </div>

    <div v-if="this.wordSearch == ''" v-bind:key="2">
      <transition-group name="custom-classes-transition" enter-active-class="animated bounceInLeft" leave-active-class="animated bounceOutRight">
        <div class="list-group" v-for="item in items" v-bind:key="item">
          <a href="#" class="list-group-item list-group-item-action flex-column align-items-start active">
            <div class="d-flex w-100 justify-content-between">
              <h5 v-if="item.state==false" >
                <img src="../assets/off.svg" style="width:'30px'" v-on:click="changeToComplete(item)"> {{item.noteName}}
              </h5>
              <h5 v-else >
                <img src="../assets/on.svg" style="width:'30px'" v-on:click="changeToComplete(item)"> {{item.noteName}}
              </h5>
              <small>{{item.date}}</small> 
            </div>
            <div class="d-flex w-100 justify-content-between"> 
              <div>
                <button v-if="item.priority == 'Low'" type="button" class="btn btn-info btn-sm" >Low</button>
                <button v-else type="button" class="btn btn-secondary btn-sm" v-on:click="changeToLow(item)">Low</button>
                <button v-if="item.priority == 'Normal'" type="button" class="btn btn-warning btn-sm">Normal</button>
                <button v-else type="button" class="btn btn-secondary btn-sm" v-on:click="changeToMedium(item)" >Normal</button>
                <button v-if="item.priority == 'High'" type="button" class="btn btn-danger btn-sm">High</button>
                <button v-else type="button" class="btn btn-secondary btn-sm" v-on:click="changeToHigh(item)" >High</button>
              </div>
              <div>
                <button  type="button" class="btn btn-success btn-sm" v-on:click="deleteNote(item)">Delete</button>
              </div>
            </div>
          </a>
        </div>
      </transition-group>
    </div>
  </div>
</div>

</template>
<script>  

  export default {
    name: "note",
    props:{
        title: String
    },
    data: function () {
      return {
        items: [],
        aux: [],
        noteName: '',
        search:'',
        
      }
    },
    computed:{
      totNotes: function(){
        return this.items.length;
      },
      totCompleted: function(){
        let c =0;
        for(let i=0; i<this.items.length;i++){
          if(this.items[i].state==true){
          c ++;
          }
        }
        return c;
      },
       wordSearch: function() {
                if(this.search == "" ){
                    return false;
                }else{
                    return this.items.filter(item => {
                        return item.noteName.toLowerCase().indexOf(this.search.toLowerCase()) > -1;
                    }); 
                }              
            },

    },
    methods:{
      addNote: function(e) {
        if (e.keyCode === 13) {
          this.items.push({ noteName: this.noteName,  priority: "Low", date: new Date().toLocaleString(), state:false});
          this.search = '';
        }
      },
      addNoteButton: function(){
          this.items.push({ noteName: this.noteName,  priority: "Low", date: new Date().toLocaleString(), state:false});
          this.search = '';
      },
     
      orderNotes: function () {
        this.aux = [];
        for (let i = 0; i < this.items.length; i++) {
            if (this.items[i].priority === "High") {
                this.aux.push(this.items[i]);
            }
        }
        for (let i = 0; i < this.items.length; i++) {
            if (this.items[i].priority === "Normal") {
                this.aux.push(this.items[i]);
            }
        }
        for (let i = 0; i < this.items.length; i++) {
            if (this.items[i].priority === "Low") {
                this.aux.push(this.items[i]);
            }
        }
        this.items = this.aux;

      },
      changeToLow: function(item){
        item.priority = "Low";
        this.orderNotes();
        
      },
      changeToMedium: function(item){
        item.priority = "Normal";
        this.orderNotes();

      },
      changeToHigh: function(item){
        item.priority = "High";
        this.orderNotes();
       
      },
      deleteNote: function(item){
        this.items.splice(this.items.indexOf(item), 1);
        
      },
      changeToComplete: function (item) {
        if (item.state == true) {
            item.state = false;
    
        } else
            item.state = true;
       
      },
      deleteCompleted: function(){
        for(let i= this.items.length-1; i >= 0;i--){
          if(this.items[i].state==true){
            this.items.splice(i, 1);
          }
         
        }
      },
    },
    mounted(){
      if (localStorage.getItem("notes-vue-cli") != null) {
				this.items = JSON.parse(localStorage.getItem("notes-vue-cli"));
			}
       
    },
    updated: function() {
			localStorage.setItem("notes-vue-cli", JSON.stringify(this.items));
		}
     
  }
</script>
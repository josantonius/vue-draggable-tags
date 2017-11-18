<template>
   <div class="mdl-grid mdl-grid center-items">

      <a href="https://github.com/Josantonius/vue-draggable-tags.git"><img
         style="position: absolute; top: 0; right: 0; border: 0; width: 150px;"
         src="https://www.felipefialho.com/piano/assets/img/ico/fork.png"
         alt="Fork me on GitHub"></a>
         
      <div class="mdl-cell mdl-cell--12-col">
         <div class="sdt-header panel-heading">
            <h2 class="panel-title mdl-cell mdl-cell--12-col">DRAG, DROP & SORT TAGS</h2>
         </div>
      </div>

      <div id="sdt-selected-tags" class="mdl-cell mdl-cell--3-col">
         <div class="panel-body sdt-teal-switch">
            <label class="mdl-switch mdl-js-switch mdl-js-ripple-effect" for="switch-1">
              <input type="checkbox" id="switch-1" v-model="selected.editable" class="mdl-switch__input">
              <span class="mdl-switch__label">Enable drag and drop</span>
            </label>
            <label class="mdl-switch mdl-js-switch mdl-js-ripple-effect" for="switch-2">
              <input type="checkbox" v-model="selected.droppable" id="switch-2" class="mdl-switch__input">
              <span class="mdl-switch__label">Enable dropping</span>
            </label>
            <label class="mdl-switch mdl-js-switch mdl-js-ripple-effect" for="switch-3">
              <input type="checkbox" v-model="selected.cloneable" id="switch-3" class="mdl-switch__input">
              <span class="mdl-switch__label">Enable cloning</span>
            </label>
         </div>
         <button class="sdt-sortable-btn sdt-sort-button mdl-button mdl-button--fab mdl-button--mini-fab mdl-js-button mdl-button--raised mdl-js-ripple-effect mdl-button--accent mdl-color--teal-100" @click="orderList(selectedTags)">
            <i class="material-icons  sdt-color-teal"><sort-alphabetical-icon /></i>
         </button>
         <div  class="mdl-cell mdl-cell--12-col mdl-card mdl-shadow--2dp mdl-color--teal-500">
            <draggable element="span" v-model="selectedTags" :options="selectedTagsOptions" :move="onMove" @start="isDragging=true" @end="isDragging=false"> 
               <transition-group  type="transition"  name="'flip-list'" class="mdl-list dragArea">
                  <li class="sdt-tag mdl-cell mdl-cell--12-col sdt-tag-item mdl-list__item mdl-card mdl-shadow--2dp" v-for="(element, index) in selectedTags" :key="element.tag">
                     <span class="mdl-list__item-primary-content">
                        <i v-bind:class="[element.fixed ? 'sdt-color-red' : 'sdt-color-gray']" class="material-icons sdt-tag-icon sdt-tag-icon-left" v-on:click="element.fixed = !element.fixed"><lock-icon /></i>
                        {{element.tag}}
                     </span>
                     <i class="material-icons sdt-tag-icon sdt-tag-icon-right sdt-color-teal" v-on:click="moveTag(index, 'selected', 'available')"><arrow-right-icon /></i>
                  </li> 
               </transition-group>
            </draggable>
         </div>
      </div>

      <div id="sdt-available-tags" class="mdl-cell mdl-cell--3-col">
         <div class="panel-body sdt-indigo-switch">
            <label class="mdl-switch mdl-js-switch mdl-js-ripple-effect" for="switch-4">
              <input type="checkbox" id="switch-4" v-model="available.editable" class="mdl-switch__input">
              <span class="mdl-switch__label">Enable drag and drop</span>
            </label>
            <label class="mdl-switch mdl-js-switch mdl-js-ripple-effect" for="switch-5">
              <input type="checkbox" v-model="available.droppable" id="switch-5" class="mdl-switch__input">
              <span class="mdl-switch__label">Enable dropping</span>
            </label>
            <label class="mdl-switch mdl-js-switch mdl-js-ripple-effect" for="switch-6">
              <input type="checkbox" v-model="available.cloneable" id="switch-6" class="mdl-switch__input">
              <span class="mdl-switch__label">Enable cloning</span>
            </label>
         </div>
         <button class="sdt-sortable-btn sdt-sort-button mdl-button mdl-button--fab mdl-button--mini-fab mdl-js-button mdl-button--raised mdl-js-ripple-effect mdl-button--accent mdl-color--indigo-100" @click="orderList(availableTags)">
            <i class="material-icons sdt-color-indigo"><sort-alphabetical-icon /></i>
         </button>
         <div  class="mdl-cell mdl-cell--12-col mdl-card mdl-shadow--2dp mdl-color--indigo-500">
            <draggable class="mdl-list dragArea" v-model="availableTags" :options="availableTagsOptions" :move="onMove" @start="isDragging=true" @end="isDragging=false"> 
               <transition-group type="transition" :name="'flip-list'">
                  <li class="sdt-tag mdl-cell mdl-cell--12-col sdt-tag-item mdl-list__item mdl-card mdl-shadow--2dp" v-for="(element, index) in availableTags" :key="element.tag">
                     <span class="mdl-list__item-primary-content">
                        <i class="sdt-tag-icon sdt-tag-icon-left material-icons sdt-color-indigo sdt-tag-icon" v-on:click="moveTag(index, 'available', 'selected')"><arrow-left-icon /></i>
                        {{element.tag}}
                     </span>
                        <i v-bind:class="[element.fixed ? 'sdt-color-red' : 'sdt-color-gray']" class="material-icons sdt-tag-icon sdt-tag-icon-right" v-on:click="element.fixed = !element.fixed"><lock-icon /></i>
                  </li> 
               </transition-group>
            </draggable>
         </div>
      </div>

      <div  class="sdt-json-section mdl-cell mdl-cell--3-col mdl-card sdt-color-teal mdl-color--teal-50">
         <pre class="sdt-json-pre">{{selectedTagsOptionsString}}</pre>
      </div>

      <div class="sdt-json-section mdl-cell mdl-cell--3-col mdl-card mdl-card sdt-color-indigo mdl-color--indigo-50">
         <pre class="sdt-json-pre">{{availableTagsString}}</pre>
      </div>

      <div class="mdl-cell mdl-cell--12-col">
         <footer class="sdt-footer">
            <p class="panel-title mdl-cell mdl-cell--12-col">Created with <a href="https://getmdl.io/" class="mdl-color-text--teal-700">MDL</a>, <a href="https://vuejs.org/" class="mdl-color-text--teal-700">Vue.js</a> and <a href="https://github.com/SortableJS/Vue.Draggable" class="mdl-color-text--teal-700">Vue.Draggable</a>.<br>© 2017 <a href="https://github.com/Josantonius" class="mdl-color-text--teal-700">Josantonius</a></p>
         </footer>
      </div>
   </div>
</template>

<script>
import draggable from 'vuedraggable'
import 'icons/styles.css'
import LockIcon from 'icons/lock'
import ArrowLeftIcon from 'icons/arrow-left'
import ArrowRightIcon from 'icons/arrow-right'
import SortAlphabeticalIcon from 'icons/sort-alphabetical'

export default {

  name: 'vue-draggable-tags',
  components: {
    draggable,
    LockIcon,
    ArrowLeftIcon,
    ArrowRightIcon,
    SortAlphabeticalIcon,
  },
  data () {
    return (typeof SDT !== 'undefined') ? SDT : {
      selectedTags: [],
      availableTags: [
        { tag: "Go", slug: "go", fixed: true },
        { tag: "PHP", slug: "php", fixed: true },
        { tag: "R", slug: "r", fixed: true },
        { tag: "Kotlin", slug: "kotlin", fixed: false },
        { tag: "Java", slug: "java", fixed: false },
        { tag: "C++", slug: "c--", fixed: false },
        { tag: "JavaScript", slug: "javascript", fixed: false },
        { tag: "C#", slug: "c-", fixed: false },
        { tag: "C", slug: "c", fixed: false },
        { tag: "Python", slug: "phyton", fixed: false }
      ],
      selected: {
        editable: true,
        cloneable: false,
        droppable: true,
      },
      available: {
        editable: true,
        cloneable: false,
        droppable: true,
      },
      isDragging: false,
      delayedDragging:false
    }
  },
  methods:{
    orderList (datalist) {
      datalist.sort((one,two) =>{
        return one.tag.localeCompare(two.tag); })
    },
    moveTag (index, fromPlace, toPlace) {
      var fromList = this[fromPlace + 'Tags'];
      var toList = this[toPlace + 'Tags'];
      var currentTag = fromList[index];
      fromPlace = this[fromPlace];
      toPlace = this[toPlace];
      if (!currentTag.fixed && fromPlace.editable && toPlace.editable && toPlace.droppable) {
        if (fromPlace.cloneable || toList.filter(function(e) { 
          return e.tag == currentTag.tag; }).length === 0) {
          setTimeout(function() {
            toList.push(currentTag);
          },300);
        }
        if (!fromPlace.cloneable) {
          fromList.splice(index, 1);
        }
      }
    },
    onMove ({relatedContext, draggedContext}) {
      const relatedElement = relatedContext.element;
      const draggedElement = draggedContext.element;
      return (!relatedElement || !relatedElement.fixed) && !draggedElement.fixed
    },
    getOptions (place) {
      return  {
        animation: 0,
        group:{
          name:'labels',
          pull: this[place].cloneable ? 'clone' : 'move',
          put:  this[place].droppable
        },
        disabled: !this[place].editable,
        ghostClass: 'ghost'
      };
    }
  },
  computed: {
    selectedTagsOptions () {
      return this.getOptions('selected');
    },
    availableTagsOptions () {
      return this.getOptions('available');
    },
    selectedTagsOptionsString(){
      return JSON.stringify(this.selectedTags, null, 2);  
    },
    availableTagsString(){
      return JSON.stringify(this.availableTags, null, 2);  
    }
  },
  watch: {
    isDragging (newValue) {
      if (newValue){
        this.delayedDragging= true
        return
      }
      this.$nextTick( () =>{
           this.delayedDragging =false
      })
    }
  }
}
</script>

<style>
.sdt-footer p {
  bottom: 0;
  left: 0;
  right: 0;
  padding: 2rem 0rem 3rem;
  position: absolute;
  text-align: center;
  font-size: 16px;
}
.sdt-footer a {
  text-decoration: none;
}
.flip-list-move {
  transition: transform 0.5s;
}
.flip-list-item {
  display: inline-block;
  margin-right: 10px;
}
.flip-list-enter-active, .flip-list-leave-active {
  transition: all 0.3s;
}
.flip-list-enter {
  opacity: 0;
  transform: translateY(-50px);
}
.flip-list-leave-to {
  opacity: 0;
  transform: translateX(-50px);
}
.no-move {
  transition: transform 0s;
}
.ghost {
  opacity: .5;
  background: #C8EBFB;
}
.dragArea {
  min-height: 528px;
  padding: 12px 12px !important;
}
.mdl-grid.center-items {
  justify-content: center;
}
.panel-body {
  margin-left: 10px;
  margin-bottom: -39px;
}
.sdt-tag-icon {
  cursor: pointer;
}
 .sdt-tag-icon-left {
  margin-right: 10px;
}
 .sdt-tag-icon-right {
  margin-left: 10px;
}
.sdt-sortable-btn {
  margin: 10px 10px !important;
}
.sdt-json-section {
  padding: 0px 13px;
  height: 553px;
  top: 93px;
}
.sdt-json-section:hover {
  overflow: auto !important;
}
.sdt-json-pre {
  font-family: monospace;
  font-weight: lighter;
  font-size: 12px;
}
.sdt-header {
  text-align: center;
  color: rgba(0, 0, 0, 0.64);
  margin-bottom: 45px;
  margin-top: 30px;
}
.sdt-control-tag {
  visibility: hidden !important;
  width: 5px !important;
  background: transparent !important;
  box-shadow: none !important;
  color: transparent !important;
  margin-bottom: -64px !important;
}
.sdt-sort-button {
  float: right;
  float: right;
  z-index: 9;
  bottom: -29px;
  left: 14px;
  width: 100%;
}
.sdt-tag {
 cursor: -webkit-grab !important;
  padding: 8px !important;
  min-height: 5px !important;
}
.sdt-tag:hover {
  box-shadow: 0 8px 10px 1px rgba(0,0,0,.14), 0 3px 14px 2px rgba(0,0,0,.12), 0 5px 5px -3px rgba(0,0,0,.2);
}
.sdt-color-red {
   fill: #f44336;
   color: #f44336;
}
.sdt-color-gray {
   fill: #e0e0e0;
   color: #e0e0e0;
}
.sdt-color-teal {
   fill: #009688;
   color: #009688;
}
.sdt-color-indigo {
   fill: #3f51b5;
   color: #3f51b5;
}
.sdt-teal-switch > .mdl-switch.is-checked .mdl-switch__thumb {
    background-color: #009688 !important;
}
.sdt-teal-switch > .mdl-switch.is-checked .mdl-switch__track {
    background: rgba(0, 150, 136, 0.51) !important;
}
.mdl-switch__ripple-container .mdl-ripple {
    background: #009688 !important;
}
</style>

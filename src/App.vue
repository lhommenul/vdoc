<template>

  <Doc 
    :docData="docData"
    :docProps="docProps"
    :docMethods="docMethods"
    :docComputed="docComputed"
  >
    <HelloWorld></HelloWorld>
  </Doc>

</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import Doc from "./components/Doc.vue";
export default {

  // COMPONENTS
  components : {
    HelloWorld
    ,Doc
  }

  // DATA
  ,data() {
    return {
      docData : {
        title : "Data"
        ,items : [ // List of data used inside the component
          {
            name : "msg"
            ,type : String
            ,value : null
            ,description : "je suis la description de la var"
          }
        ]
        ,footer : "Footer data"
      }
      ,docProps : {
        title : "Props"
        ,items : [ // List of data used inside the component
          
        ]
        ,footer : "Footer props"
      }
      ,docComputed : {
        title : "Computed"
        ,items : [ // List of data used inside the component
          
        ]
        ,footer : "Footer Computed"
      }
      ,docMethods : {
        title : "Methods"
        ,items : [ // List of data used inside the component
          
        ]
        ,footer : "Footer Methods"
      }
    }
  },
}
</script>

<style scoped>

</style>

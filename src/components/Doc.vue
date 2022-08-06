<template>
  <section class="doc-main-container">

    <!-- COMPONENT TO DOCUMENT -->
    <div ref="doc" >
      <slot>
        You can place the component inside the doc balises
      </slot>
    </div>


    <div class="doc-container">

      <!-- DATA DOC -->
      <section class="card-doc" v-if="generatedDocData" >
        <!-- DATA TITLE -->
        <header>
          <h2>{{ generatedDocData.title }}</h2>
        </header>
        <!-- DATA DOCUMENTATION BODY -->
        <div class="body" >
          <table>
            <tr>
              <th>name</th>
              <th>value</th>
              <th>description</th>
              <th> type</th>
            </tr>
            <tr class="body-item" v-for="(item, index) in generatedDocData.items" :key="index" >
              <td> <em> {{ item.name }} </em> </td> 
              <td> {{ item.value }} </td> 
              <td> {{ item.description }} </td> 
              <td> {{ item.type }} </td> 
            </tr>
          </table>
        </div>
        <!-- DATA FOOTER -->
        <footer>
          <h2>{{ generatedDocData.footer }}</h2>
        </footer>
      </section>

      <!-- DATA PROPS -->
      <section class="card-doc" v-if="generatedDocProps" >
        <!-- PROPS TITLE -->
        <header>
          <h2>{{ generatedDocProps.title }}</h2>
        </header>
        <!-- PROPS DOCUMENTATION BODY -->
        <div class="body" >
          <table>
            <tr>
              <th>name</th>
              <th>value</th>
              <th>description</th>
              <th> type</th>
            </tr>
            <tr class="body-item" v-for="(item, index) in generatedDocProps.items" :key="index" >
              <td> <em> {{ item.name }} </em> </td> 
              <td> {{ item.value }} </td> 
              <td> {{ item.description }} </td> 
              <td> {{ item.type }} </td> 
            </tr>
          </table>
        </div>
        <!-- PROPS FOOTER -->
        <footer>
          <h2>{{ generatedDocProps.footer }}</h2>
        </footer>
      </section>

      <!-- DATA COMPUTED -->
      <section class="card-doc" v-if="generatedDocComputed" >
        <!-- PROPS TITLE -->
        <header>
          <h2>{{ generatedDocComputed.title }}</h2>
        </header>
        <!-- PROPS DOCUMENTATION BODY -->
        <div class="body" >
          <table>
            <tr>
              <th>name</th>
              <th>value</th>
              <th>description</th>
              <th> type</th>
            </tr>
            <tr class="body-item" v-for="(item, index) in generatedDocComputed.items" :key="index" >
              <td> <em> {{ item.name }} </em> </td> 
              <td> {{ item.value }} </td> 
              <td> {{ item.description }} </td> 
              <td> {{ item.type }} </td> 
            </tr>
          </table>
        </div>
        <!-- PROPS FOOTER -->
        <footer>
          <h2>{{ generatedDocComputed.footer }}</h2>
        </footer>
      </section>

      <!-- DATA METHODS -->
      <section class="card-doc" v-if="generatedDocMethods" >
        <!-- PROPS TITLE -->
        <header>
          <h2>{{ generatedDocMethods.title }}</h2>
        </header>
        <!-- PROPS DOCUMENTATION BODY -->
        <div class="body" >
          <table>
            <tr>
              <th>name</th>
              <th>value</th>
              <th>description</th>
              <th> type</th>
            </tr>
            <tr class="body-item" v-for="(item, index) in generatedDocMethods.items" :key="index" >
              <td> <em> {{ item.name }} </em> </td> 
              <td> {{ item.value }} </td> 
              <td> {{ item.description }} </td> 
              <td> {{ item.type }} </td> 
            </tr>
          </table>
        </div>
        <!-- PROPS FOOTER -->
        <footer>
          <h2>{{ generatedDocMethods.footer }}</h2>
        </footer>
      </section>

    </div>

  </section>
</template>

<script>

export default {

  // PROPS 
  props :{
    docData : {
      type : Object
      ,default : undefined
    }
    ,docProps : {
      type : Object
      ,default : undefined
    }
    ,docComputed : {
      type : Object
      ,default : undefined
    }
    ,docMethods : {
      type : Object
      ,default : undefined
    }
  }

  // DATA
  ,data() {
    return {
      generatedDocData : null   
      ,generatedDocProps : null   
      ,generatedDocComputed : null
      ,generatedDocMethods : null
    }
  }

  // MOUNTED
  ,created() {
    this.getDocBody()
  }

  // METHODS
  ,methods: {

    getDocBody(){

      this.$slots.default().forEach( slot => {

        const existingValues = slot.type;
        
        const [
          data
          ,props
          ,computed
          ,methods
        ] = [ existingValues.data(), existingValues.props, existingValues.computed, existingValues.methods ]

        this.generateDataDoc( data )

        this.generatePropsDoc( props )

        this.generateComputedDoc( computed )

        this.generateMethodsDoc( methods )

      })
    } 

    ,generateDataDoc( data ){ // Dev documentation

      const defaultDocData = {
        title : undefined
        ,items : [ // List of data used inside the component
        ]
        ,footer : undefined
      }
      
      const dataDoc = Object.entries(data).reduce(( acc, [ key, val ], i )=>{

        let match = acc.items.filter(( { name } ) => name === key )[0];

        const docRow = { // GENERATE A NEW DOCUMENTION FOR EACH KEY INSIDE THE DATA
           name        : key  
          ,value       : match?.value                     ?? val
          ,type        : this.replaceType( match?.type )  ?? typeof val
          ,description : match?.description               ?? ""
        }

        if ( !match ) acc.items.push( docRow )
        else Object.entries( docRow ).forEach(([k,v]) => match[k] = v) // IF THE DOCUMENT ALREADY EXIST FOR THIS ROW CREATE MODIFY THE EXISTING ONE
      
        return acc;

      }, this.docData ?? defaultDocData )

      this.generatedDocData = dataDoc;

    }

    ,generatePropsDoc( props ){

      const defaultDocProps = {
        title : undefined
        ,items : [ // List of data used inside the component
        ]
        ,footer : undefined
      }
      
      const propsDoc = Object.entries(props).reduce(( acc, [ key, val ], i )=>{

        let match = acc.items.filter(( { name } ) => name === key )[0];

        const docRow = { // GENERATE A NEW DOCUMENTION FOR EACH KEY INSIDE THE PROPS
           name        : key  
          ,value       : match?.value                     ?? val.default ?? 'undefined'
          ,type        : this.replaceType( match?.type )  ?? typeof val.default
          ,description : match?.description               ?? ""
        }

        if ( !match ) acc.items.push( docRow )
        else Object.entries( docRow ).forEach(([k,v]) => match[k] = v) // IF THE DOCUMENT ALREADY EXIST FOR THIS ROW CREATE MODIFY THE EXISTING ONE
      
        return acc;

      }, this.docProps ?? defaultDocProps )

      this.generatedDocProps = propsDoc;


    }

    ,generateComputedDoc( computed ){

      if ( !computed ) return;

      const defaultDocComputed = {
        title : undefined
        ,items : [ // List of data used inside the component
        ]
        ,footer : undefined
      }
      
      const dataComputed = Object.entries(computed).reduce(( acc, [ key, val ], i )=>{

        let match = acc.items.filter(( { name } ) => name === key )[0];

        const docRow = { // GENERATE A NEW DOCUMENTION FOR EACH KEY INSIDE THE DATA
           name        : key  
          ,value       : match?.value                     ?? val
          ,type        : this.replaceType( match?.type )  ?? typeof val
          ,description : match?.description               ?? ""
        }

        if ( !match ) acc.items.push( docRow )
        else Object.entries( docRow ).forEach(([k,v]) => match[k] = v) // IF THE DOCUMENT ALREADY EXIST FOR THIS ROW CREATE MODIFY THE EXISTING ONE
      
        return acc;

      }, this.docComputed ?? defaultDocComputed )

      this.generatedDocComputed = dataComputed;      

    }

    ,generateMethodsDoc( methods ){

      if ( !methods ) return;

      const defaultDocMethods = {
        title : undefined
        ,items : [ // List of data used inside the component
        ]
        ,footer : undefined
      }
      
      const dataMethods = Object.entries(methods).reduce(( acc, [ key, val ], i )=>{

        let match = acc.items.filter(( { name } ) => name === key )[0];

        const docRow = { // GENERATE A NEW DOCUMENTION FOR EACH KEY INSIDE THE DATA
           name        : key  
          ,value       : match?.value                     ?? 'undefined'
          ,type        : this.replaceType( match?.type )  ?? typeof val
          ,description : match?.description               ?? ""
        }

        if ( !match ) acc.items.push( docRow )
        else Object.entries( docRow ).forEach(([k,v]) => match[k] = v) // IF THE DOCUMENT ALREADY EXIST FOR THIS ROW CREATE MODIFY THE EXISTING ONE
      
        return acc;

      }, this.docMethods ?? defaultDocMethods )

      this.generatedDocMethods = dataMethods;      

    }

    ,replaceType( type ){ // WHEN A USER PROVIDE TYPE REPLACE IT BY A STRING

      if ( !type || typeof type === 'string' ) return type
      else if ( !Array.isArray(type) ) return type.name.toLowerCase()
      return type.map( t => {
        if ( t === null ) return 'null'
        return t.name.toLowerCase()
      })

    }

    ,generateDocPage(){

    }

  },
  
}
</script>

<style scoped>
.card-doc{
  --pad : 0.4rem;
  background-color: whitesmoke;
  border-radius: 0.4rem;
}
.card-doc header{
  padding: var( --pad );
  border: dashed rgba(128, 128, 128, 0.4) 1px;
  border-width:  0px 0px 1px 0px;
}
.card-doc .body table td
,.card-doc .body table th{
  padding: 1rem;
}

.card-doc footer{
  padding: var( --pad );
  border: dashed rgba(128, 128, 128, 0.4) 1px;
  border-width: 1px 0px 0px 0px;
}

.doc-container{
  display: grid;
  row-gap: 2rem;
}
</style>
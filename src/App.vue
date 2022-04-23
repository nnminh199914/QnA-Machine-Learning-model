<template>
<hello-world></hello-world>
<button @click ="toggledemo" type="submit" class="btn btn-outline-secondary col-md-6 " > 
  <div v-if= "language=='(English)'" >Demo </div>
  <div v-if= "language=='(Deutsch)'" >Beispiel</div>
</button>



<div class="d-flex p-2 text-align-center">
  <div class="radio col-md-3 offset-md-3">
    <label><input id="lg" @checked="togglelanguage" type="radio" name="optradio" value="(English)" v-model="language"  checked>English</label>
  </div>
  <div class="radio col-md-3 offset-right-md-3">
    <label><input id="lg" @checked="togglelanguage" type="radio" name="optradio" value="(Deutsch)" v-model="language" >Deutsch</label>
  </div>
</div>


<form @submit.prevent="Calldata">
<div class="column">
  <div class="form-floating mb-3 col-md-6 offset-md-3">
    <textarea required="true" v-model="context" class="form-control " placeholder="Leave a comment here" id="floatingTextarea2" style="height: 400px"></textarea>
    <label for="floatingTextarea2">1.Context</label>
  </div>
  <div class="form-floating mb-3 col-md-6 offset-md-3">
    <input required="true" v-model="question" type="text" class="form-control" id="floatingInput" placeholder="name@example.com">
    <label for="floatingInput">2.question</label>
  </div>
</div>

<button @click ="activefunc" type="submit" class="btn btn-outline-secondary col-md-6 " > 
  <div v-if= "active==false" >Get answer {{ language }} </div>
  <div v-if= "active==true" >Re-run {{ language }}</div>
</button>
</form>


<div v-if="active == true" class="form-floating mb-3 col-md-6 offset-md-3" >
    <textarea  v-model="output" class="form-control " disabled placeholder="Leave a comment here" id="floatingTextarea2" style="height: 200px"></textarea>
</div>
 
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue"
import axios from 'axios';
export default {
  name: 'App',
  components: {
    HelloWorld
  },
  data(){
    return{
      context : '',
      question : '',
      output: '',
      active: false,
      language: "(English)"
    }
  },
  methods: {
    Calldata(){
      console.log(this.language);


      if(this.language == "(English)"){
        axios.post(
        "https://api-inference.huggingface.co/models/deepset/roberta-base-squad2",
        {"inputs":{"question":this.question,"context": this.context}},
      ).then(response => {
        console.log(response.data.answer);
        this.output = response.data.answer;
      });
      }

      else{
        axios.post(
        "https://api-inference.huggingface.co/models/deepset/gelectra-base-germanquad",
        {"inputs":{"question":this.question,"context": this.context}},
      ).then(response => {
        console.log(response.data.answer);
        this.output = response.data.answer;
      });
      }
    },
    activefunc(){
      this.active = true;
    },
    togglelanguage(){
      console.log(this.language);
      if(this.language == "(English)"){
        this.language == "(Deutsch)"
      }
      else{
        this.language == "(English)"
      }
    },
    toggledemo(){
      if(this.language == "(English)"){
        this.context = 'Xylitol is also known as birch sugar. Xylitol tastes just as sweet as sugar, and you can replace it one-to-one in recipes. But why should you do that? Birch sugar has some positive advantages over table sugar. On the one hand, it is tooth-friendly and even has a non-cariogenic effect, i.e. it does not cause caries. Secondly, xylitol is particularly popular because of its low glycemic index. It has a glycemic index of 7 to 11, while that of regular sugar is 65 to 100. So if you want to consciously avoid the negative properties of sugar, but not its sweetness, you should give xylitol a try. However, in moderation, because in high doses it can have a laxative effect. Birch sugar should also be kept away from dogs, as it can even be fatal for the four-legged friends.'
        this.question = 'What is the advantage of birch sugar?'
      }

      else{
        this.context = 'Das Apache Lucene-Projekt entwickelt Open-Source-Such-Software. Das Projekt veröffentlicht eine Kern-Suchbibliothek, genannt Lucene Core, sowie den Solr-Suchserver. Solr ist ein hochleistungsfähiger Suchserver, der auf Lucene Core aufbaut. Solr ist hoch skalierbar und bietet vollständig fehlertolerante verteilte Indizierung, Suche und Analyse. Er stellt die Funktionen von Lucene über einfach zu verwendende JSON/HTTP-Schnittstellen oder native Clients für Java und andere Sprachen zur Verfügung.'
        this.question = 'Wie kann ich den Suchserver ansprechen?'
      }
    }

  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 90px;
  color: #778DA9;
}
</style>

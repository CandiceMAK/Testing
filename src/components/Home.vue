<template>
<nav class="navbar" role="navigation" aria-label="main navigation">
  <div class="navbar-brand">
    <a class="navbar-item" href="https://bulma.io">
      <h2 class="has-text-primary has-text-weight-bold is-size-2">Manta</h2>
    </a>
    <div class="navbar-item ">
    <div role="button" class="navbar-item has-dropdown is-hoverable" aria-label="menu" aria-expanded="false" data-target="navbar-dropdown">
        <div class="navbar-dropdown">
        <a class="navbar-item">
        Home
      </a>

      <a href="#disorder" class="navbar-item">
        Disorders and issues
      </a>
      <a href="#addiction" class="navbar-item">
        Addiction
      </a>
      <a a href="#stats" class="navbar-item">
        Statistics
      </a>
    </div>
    </div>
    </div>
  </div>

  <div id="navbarBasicExample" class="navbar-menu">
    <div class="navbar-start">
      <a class="navbar-item">
        Home
      </a>

      <a href="#disorder" class="navbar-item">
        Disorders and issues
      </a>
      <a href="#addiction" class="navbar-item">
        Addiction
      </a>
      <a a href="#stats" class="navbar-item">
        Statistics
      </a>
    
    </div>

    <div class="navbar-end">
      <div class="navbar-item">
        <div class="buttons">
          <a class="button is-primary">
            <strong>Sign up</strong>
          </a>
          <a class="button is-light">
            Log in
          </a>
        </div>
      </div>
    </div>
  </div>
</nav>
<div class="columns">
  <div class="column">
    <section class="hero">
    <div class="hero-body">
        <p class="title">
        Mental Health
        </p>
        <br />
        <p class="subtitle has-text-justified">
        Mental health difficulties like anxiety, depression, and post-traumatic stress disorder can lead to substance abuse problems and, conversely, using substances can worsen mental health conditions in some people. Dual-diagnosis treatment can help address both issues simultaneously.
        </p>
    </div>
</section>
  </div>
  <div class="column">
     <img src="../assets/Mental-Health-2-1.jpg">
  </div>
</div>
<br />
<p class="title has-text-primary">
    Statistics
</p>
<section id="stats" class="container columns">
    <div class="column column is-7 content is-medium has-text-justified ml-5">
        <h2>Disorder types bar chart</h2>

            <table class="summaries">
            <tr>
            <td>
                <div class="label">Total</div>
                <div class="value" mc:edit="total">717 m</div>
            </td>
            <td class="critical">
                <div class="label">Disorders</div>
                <div class="value" mc:edit="critical">5</div>
            </td>
            
            </tr>
            </table>
        <table v-if="!typesLoading && typesData && typesData.length"  class="barchart" cellpadding="1" cellspacing="0">
        <tr>
            <td v-for="item of typesData" v-bind:key="item.number"> 
                <span class="label">{{item.number}} million</span>
                <div class="bar" :style="`height:${item.percentage}%`"></div>
            </td>
        </tr>
        <tr>
            <td v-for="item of typesData" v-bind:key="item.number"> 
                {{item.disorder}}
            </td>
        </tr>
        </table>
        <div v-if="typesLoading">
        Loading data...
        </div>
        <div v-if="typesError">
        Failed to get data. Please try again!
        </div>
    </div>
    <div class="column column is-5 content is-medium has-text-justified">
            <h4>Mental health disorder prevalence</h4>
        <p>
            For 2017 this study estimates that 792 million people lived with a mental health disorder. This is slightly more than one in ten people globally (10.7%)
            Mental health disorders are complex and can take many forms. 
            The underlying sources of the data presented in this entry apply specific definitions (which we describe in each relevant section), 
            typically in accordance with WHO’s International Classification of Diseases (ICD-10). 
            This broad definition incorporates many forms, including depression, anxiety, bipolar, eating disorders and schizophrenia.
        </p>
    </div>
</section>
<footer class="footer">
  <div class="content has-text-centered">
    <p>
      <strong>Manta</strong> Copyright @2021. Made with <strong>Love</strong>
      
    </p>
  </div>
</footer>

</template>

<script>
import { ref, onMounted } from "vue";

export default {
    name:"Home",

    setup() {

    var typesData = ref(null);
    var typesLoading = ref(true);
    var typesError = ref(null);

    function fetchTypesData(){
         typesLoading.value = true;
        // I prefer to use fetch
        return fetch('https://api.npoint.io/a8eba92f2aff8fddf859', {
            method: 'get',
            headers: {
            'content-type': 'application/json'
            }
        })
            .then(res => {
            // a non-200 response code
            if (!res.ok) {
                // create error instance with HTTP status text
                const error = new Error(res.statusText);
                error.json = res.json();
                throw error;
            }

            return res.json();
            })
            .then(json => {
            // set the response data
            console.log(json)
            typesData.value = json;
            
            })
            .catch(err => {
            addError.value = err;
            // In case a custom JSON error response was provided
            if (err.json) {
                return err.json.then(json => {
                // set the JSON response message
                error.value.message = json.message;
                });
            }
            })
            .then(() => {
            typesLoading.value = false;
            });
    }

    onMounted(() => {
      fetchTypesData();
    });

    return {
      typesData,
      typesLoading,
      typesError
    };
  }
}
</script>

<style>
    
/* List */
table.errorlist .counter{
  text-align: right;
}
table.errorlist .counter span{
  background-color: #eee;
  border-radius: 2px;
  padding: 1px 5px;
}
/* Summaries*/
table.summaries td{
  padding-right: 40px;
}
table.summaries td.critical{
  color: #e6614f;
}
table.summaries div.value{
  font-size: 40px;
  margin-top: 10px;
}
/* Bar Chart */
.barchart{
  font-size: 9px;
  line-height: 15px;
    table-layout: fixed;
    text-align:center; 
    width: 100%;
    height: 200px;
    margin-top: -150px;
}
.barchart tr:nth-child(1) td{
    vertical-align:bottom;
    height:350px;
  }
  
.barchart .bar{
    background: #7957d5;
    padding: 10px 2px 0;
}
  
.barchart .label{
    background-color: black;
  margin-top: -30px;
  padding: 0 3px;
  color: white;
  border-radius: 4px;
}
</style>
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

<p class="title has-text-primary">
    Addiction
</p>
<section id="addiction" class="container columns"> 
    <div class="content is-medium has-text-justified column column is-6 ml-5">
    <h3>What is addiction?</h3>
    <p>the repeated involvement with a substance or activity, despite the substantial 
        harm it now causes, because that involvement was (and may continue to be) pleasurable and/or valuable.

    </p>
    <h4>What Causes Addiction?</h4>
    <p>There are many theories on what causes addiction. Some contend
         that it is genetically determined, while at 
        the other end of the spectrum, others argue that it is caused
         by purely environmental factors, like a turbulent childhood. Some of the theories include;</p>
    <ol>
      <li>Biological Perspectives..</li>
      <li>Psychological Perspectives.</li>
      <li>Moral & Spiritual Perspectives..</li>
      <li>12-Step Perspectives.</li>
      <li>Cognitive-Behavioral/Social Learning Theories.</li>
    </ol>
  </div>
  <div class="column column is-6 content is-medium has-text-justified">
      <h4>Data on Addiction</h4>
      <p>The following table shows Heroin addiction and deaths from 2002 to 2013
          SOURCE: National Survey on Drug Use and Health (NSDUH), 2002-2013. SOURCE: Multiple Cause of Death Files from the National Vital Statistics System, 2002-2013
    </p>
    <table class="table has-text-centered">
    <thead>
        <th>YEAR</th>
        <th>PER 1000 PEOPLE</th>
        <th>PER 100000 PEOPLE</th>
    </thead>
    <tbody v-if="!addLoading && addictionData && addictionData.length">
    <tr class="is-selected" v-for="item of addictionData" v-bind:key="item.year">
        <td>{{item.year}}</td>
        <td>{{item.heroin_addiction_per_1_000_people}}</td>
        <td>{{item.heroin_related_overdose_deaths_per_100_000_people}}</td>
    </tr>
    </tbody>
    <div v-if="addLoading">
    Loading data...
    </div>
    <div v-if="addError">
    Failed to get data. Please try again!
    </div>
    </table>
    
  </div>
</section>
<br />
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

    var addictionData = ref(null);
    var addLoading = ref(true);
    var addError = ref(null);

    function fetchAddictionData(){
         addLoading.value = true;
        // I prefer to use fetch
        return fetch('https://api.npoint.io/be8d33e0b6fddae21cfa', {
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
            addictionData.value = json;
            
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
            addLoading.value = false;
            });
    }

    onMounted(() => {
      fetchAddictionData();
    });

    return {
      addictionData,
      addLoading,
      addError,
    };
  }
}
</script>

<style>
    
</style>
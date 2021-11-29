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
Disorders and issues
</p>
<section id="disorder" v-if="!loading && data && data.length" class="columns container is-fluid is-flex-wrap-wrap">
    <div v-for="item of data" v-bind:key="item.title" class="column is-4">
        <div class="card">
            <header class="card-header">
                <p class="card-header-title">
                 {{item.title}}
                </p>
            </header>
            <figure class="image is-4by3">
            <img :src="item.imgUrl" alt="Placeholder image">
            </figure>
            
            <div class="card-content">
            <div class="content">
            {{item.content}}
            </div>
        </div>
        </div>
    </div>
</section>
<div v-if="loading">
    Loading data...
</div>
<div v-if="error">
    Failed to get data. Please try again!
</div>
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
    const data = ref(null);
    const loading = ref(true);
    const error = ref(null);

    function fetchData() {
            loading.value = true;
        // I prefer to use fetch
        // you can use use axios as an alternative
        return fetch('https://api.npoint.io/6529161687e44cef391a', {
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
            data.value = json;
            
            })
            .catch(err => {
            error.value = err;
            // In case a custom JSON error response was provided
            if (err.json) {
                return err.json.then(json => {
                // set the JSON response message
                error.value.message = json.message;
                });
            }
            })
            .then(() => {
            loading.value = false;
            });
    }

    onMounted(() => {
      fetchData();
    });

    return {
      data,
      loading,
      error
    };
  }
}
</script>

<style>
    
</style>
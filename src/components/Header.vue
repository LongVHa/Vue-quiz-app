<template>
  <div>
  <b-nav tabs>
    <b-nav-item disable><b>Fancy Quiz App</b></b-nav-item>
    <b-nav-item disable>Counter: {{ numCorrect }} / {{ numTotal}}</b-nav-item>
  </b-nav>
  </div>
</template>

<script>
export default {
  props:[
    'numCorrect',
    'numTotal'
  ]
}
</script>
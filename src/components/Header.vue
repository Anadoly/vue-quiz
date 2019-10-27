<template>
  <div>
    <b-nav tabs> 
      <b-nav-item disabled>
        <b>Fancy Quiz App </b>
      </b-nav-item>
      <b-nav-item disabled>
        Counter: {{ numOfCorrectAnswers }}/{{ numOfTotal }}
      </b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
 props: {
   numOfTotal: {
     type: Number,
     required: true,
   },
   numOfCorrectAnswers: {
     type: Number,
     required: true,
   }
 } 
}
</script>
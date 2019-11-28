<template>
    <div class="header">
        <h1>Quiz App</h1>
        <p>Counter: {{numCorrect}}/{{numTotal}}</p>   
    </div>
</template>

<script>
export default {
    props: [
        'numCorrect',
        'numTotal'
    ]
}
</script>

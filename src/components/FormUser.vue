<script setup>
</script>

<template>
    <form>
        <label for="nome">nome</label>
        <input type="text" name="nome">
        <label for="email">email</label>
        <input type="email" name="email">
        <label for="idade">idade</label>
        <input type="number" name="idade">
    </form>
</template>

<style>
form {
    display: flex;
    flex-direction: column;
}
input {
    max-width: 15%;
}
</style>
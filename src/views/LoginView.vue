<template>
    <div class="login">
        <Header/>
        <login/>
    </div>
</template>
<script>
import Header from '../components/Header.vue'
import Login from '../components/Login.vue'

export default{
    name: 'LoginView',
    components: {
        Header,
        Login
    }

}

</script>
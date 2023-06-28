<template>
    <div class="graph">
        <Header/>
        <Footer/>
    </div>
    
</template>

<script>
import Header from '@/components/Header.vue';
import Footer from '@/components/Footer.vue';

export default{
    name: 'GraphView',
    components: {
        Header,
        Footer
    }
}
</script>
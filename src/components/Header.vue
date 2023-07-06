<template>
    <div>
        <b-navbar toggleable="md" type="dark" class="p-3 bg-lila">
            <b-container>
                <img src="../assets/logo.png" alt="logo" height="50px" width="50px">
                <b-navbar-brand :to="{name: 'home'}">Denunciango web</b-navbar-brand>

                <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

                <b-collapse id="nav-collapse" is-nav>
                <b-navbar-nav>
                    <b-nav-item :to="{name: 'map'}" exact>Mapa de denuncias</b-nav-item>
                    <b-nav-item :to="{name: 'graph'}" exact >Grafico de denuncias</b-nav-item>
                    <b-nav-item :to="{name: 'login'}" exact>Iniciar sesion</b-nav-item>

                </b-navbar-nav>

                <!-- Right aligned nav items -->
              
                </b-collapse>
            </b-container>
           
        </b-navbar>
    </div>
</template>

<script>
    export default{
        name: 'Header'
    }
</script>

<style>
    .bg-lila {
        background-color: #6c3f70; /* Código de color para el tono lila */
        color: white; /* Opcional: Cambia el color del texto para que sea legible en el fondo lila */
    }
</style>
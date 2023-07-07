<template>
    <div class="w-100">
        <b-navbar toggleable="md" type="dark" class="p-3 bg-lila w-100 d-flex">
        <div class="mx-5 d-flex w-100 justify-content-between"> 
            <div  class="mx-5">
                <img src="../assets/logo.png" alt="logo" height="50px" width="50px">
                <b-navbar-brand :to="{name: 'home'}">Denunciango web</b-navbar-brand>
            </div>

            <div class="d-flex mx-5">
                <b-avatar src="https://i.pinimg.com/564x/de/2f/96/de2f9614d5ff43f54f97911e2d82d443.jpg" ></b-avatar>
                <b-navbar-nav>
                    <b-nav-item-dropdown right>
                        <template #button-content>
                        <em>Andres Campos</em>

                        </template>
                        <b-dropdown-item href="#" style="width: 30px;">Perfil</b-dropdown-item>
                        <b-dropdown-item href="#">Salir</b-dropdown-item>
                    </b-nav-item-dropdown>
                </b-navbar-nav>
            </div>
    

            </div>  
        </b-navbar>
    </div>  
</template>

<script>
    export default{
        name: 'HeaderAuth'
    }
</script>

<style>
    .bg-lila {
        background-color: #6c3f70; /* Código de color para el tono lila */
        color: white; /* Opcional: Cambia el color del texto para que sea legible en el fondo lila */
    }
</style>
<template>
    <div>
        <b-navbar toggleable="md" type="dark" variant="danger">
            <b-container>
                <b-navbar-brand href="#">Denunciango web</b-navbar-brand>

                <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

                <b-collapse id="nav-collapse" is-nav>
                <b-navbar-nav>
                    <b-nav-item :to="{name: 'home'}" exact>Home</b-nav-item>
                    <b-nav-item :to="{name: 'about'}">About</b-nav-item>
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
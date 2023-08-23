<template>
    <div id="align">
        <b-jumbotron id="mytasklist">
            <h2 class="text-uppercase">to-do list apps</h2>
        </b-jumbotron>
    </div>
</template>

<script>
import ToastMixin from "@/mixins/toastMixin.js";

export default {
  name: "Home",
}
</script>

<style>
#mytasklist{
    background-color: #fff;
}

#mytasklist h1{
    color: #000;
    font-weight: 700 !important;
    font-size: 11rem !important;
}

#mytasklist p{
    color: #000;
    font-weight: 400 !important;
    font-size: 20px !important;
}

#mytasklist a{
    font-family: 'Dancing Script', cursive;
    text-decoration: none;
    color: #000;
    font-weight: 600 !important;
    font-size: 24px;
}

#align{
    height: calc(100vh - 56px);
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    text-align: center;
}

@media only screen and (max-width: 768px) {
    #mytasklist h1{
    color: #000;
    font-weight: 700 !important;
    font-size: 5rem !important;
}
}

</style>


<script>
  export let onClick, number;
</script>

<style>
   .square {
     cursor: pointer;
     display: grid;
     place-items: center;
     background-color: rgb(247, 247, 247);
     border: 2px solid black;
     border-radius: 15px;
   }
   .square:hover {
     background-color: rgb(17, 17, 17);
     opacity: 0.4px;
   }

   .square p {
     font-size: 3.4em;
     font-weight: bold;
     color: rgb(0, 0, 0);
     -webkit-animation: color-animation 15s infinite alternate linear;
     -moz-animation: color-animation 15s infinite alternate linear;
     -o-animation: color-animation 15s infinite alternate linear;
   }
   @-webkit-keyframes color-animation {
     0% {
       color: #ff0000;
     }

     20% {
       color: #ff8040;
     }

     40% {
       color: #ffff00;
     }

     60% {
       color: #008000;
     }

     80% {
       color: #0000ff;
     }

     100% {
       color: #8080ff;
     }
   }

   @-moz-keyframes color-animation {
     0% {
       color: #ff0000;
     }

     20% {
       color: #ff8040;
     }

     40% {
       color: #ffff00;
     }

     60% {
       color: #008000;
     }

     80% {
       color: #0000ff;
     }

     100% {
       color: #8080ff;
     }
   }

   @-o-keyframes color-animation {
     0% {
       color: #ff0000;
     }

     20% {
       color: #ff8040;
     }

     40% {
       color: #ffff00;
     }

     60% {
       color: #008000;
     }

     80% {
       color: #0000ff;
     }

     100% {
       color: #8080ff;
     }
   }
</style>

<div class="square awesome" on:click={onClick} {number}>
  <p />
</div>
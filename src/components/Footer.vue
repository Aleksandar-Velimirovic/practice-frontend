<template>
  <footer>
    <div class="main-div">
      <div class="line-one"></div>
        <div class="footer-links">
          <ul>
            <li><a href="#">About</a></li>
            <li><a href="#">Development</a></li>
            <!-- <li><a href="#">Useful Links</a></li> -->
            <li><a href="#">FAQ</a></li>
            <li><a href="#">Contact</a></li>
            <!-- <li><a href="#">Company</a></li> -->
          </ul>
        </div>
      <div class="line-two"></div>
    </div>
  </footer>
</template>

<script>
export default {

}
</script>

<style scoped>

  .main-div {
    background-color: rgba(var(--b3f,250,250,250),1);
    /* margin-top: 400px; */
  }

   .instabook-link {
    display: flex;
    justify-content: center;
    margin-top: 50px;
  }

   .footer-links {
     display: flex;
     justify-content: center;
     flex-direction: row;
     text-decoration: none;
     margin-top: 200px;
   }

   .footer-links ul{
     display: flex;
     justify-content: space-between;
     flex-direction: row;
     gap: 30px;
     list-style-type: none;
   }

   .footer-links ul a{
    text-decoration: none;
   }

    .line-two {
      height: 2px;
      color: #66a3ff;
  }

   .line-two {
      height: 2px;
      color: #66a3ff;
  }
</style>
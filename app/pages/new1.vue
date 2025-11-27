<template>
  <div class="min-h-screen bg-[#353B3F] font-roboto py-32 px-4">
    <!-- Background Text -->
    <div class="fond">
      <span class="s1 absolute top-0 left-0 text-[15rem] font-extrabold uppercase text-[#3C4447] leading-none opacity-50">
        blog
      </span>
      <span class="s2 absolute bottom-0 right-0 text-[15rem] font-extrabold uppercase text-[#3C4447] leading-none opacity-50">
        card
      </span>
    </div>

    <!-- Card Container -->
    <div class="card relative h-[450px] w-full max-w-[900px] mx-auto bg-white shadow-[10px_10px_93px_0px_rgba(0,0,0,0.75)]">
      <!-- Thumbnail Image -->
      <div class="thumbnail absolute left-[30px] top-[-30px] h-[320px] w-[530px] overflow-hidden shadow-[10px_10px_60px_0px_rgba(0,0,0,0.75)]">
        <img
          class="left absolute left-1/2 top-1/2 h-auto w-full -translate-x-1/2 -translate-y-1/2 transform"
          src="https://cdn2.hubspot.net/hubfs/322787/Mychefcom/images/BLOG/Header-Blog/photo-culinaire-pexels.jpg"
          alt="Blog thumbnail"
        />
      </div>

      <!-- Right Content -->
      <div class="right ml-[590px] mr-5">
        <h1 class="pt-[15px] text-[1.3rem] text-[#4B4B4B]">
          Why you Need More Magnesium in Your Daily Diet
        </h1>

        <!-- Author -->
        <div class="author mt-4 flex h-[30px] w-[110px] items-center rounded-[20px] bg-[#9ECAFF]">
          <img
            class="ml-2.5 h-5 w-5 rounded-full"
            src="https://randomuser.me/api/portraits/men/95.jpg"
            alt="Author"
          />
          <h2 class="ml-auto mr-1.5 text-[0.8rem] text-white">
            Igor MARTY
          </h2>
        </div>

        <!-- Separator -->
        <div class="separator mt-2.5 border-t border-[#C3C3C3]"></div>

        <!-- Description -->
        <p class="pt-2.5 text-justify text-[0.95rem] leading-[150%] text-[#4B4B4B]">
          Magnesium is one of the six essential macro-minerals that is required by the body for energy production and synthesis of protein and enzymes. It contributes to the development of bones and most importantly it is responsible for synthesis of your DNA and RNA. A new report that has appeared in theBritish Journal of Cancer, gives you another reason to add more magnesium to your diet...
        </p>
      </div>

      <!-- Date (Inside Card) -->
      <h5 class="absolute left-[30px] bottom-[30px] text-[6rem] text-[#C3C3C3] leading-none">
        12
      </h5>
      <h6 class="absolute left-[30px] bottom-[10px] text-[2rem] text-[#C3C3C3] leading-none">
        JANUARY
      </h6>

      <!-- Social Icons -->
      <ul class="absolute bottom-[50px] left-[250px] flex gap-10">
        <li class="text-[#7B7B7B] cursor-pointer hover:text-[#1875D0] transition-colors">
          <i class="fa fa-eye fa-2x"></i>
        </li>
        <li class="text-[#7B7B7B] cursor-pointer hover:text-[#1875D0] transition-colors">
          <i class="fa fa-heart-o fa-2x"></i>
        </li>
        <li class="text-[#7B7B7B] cursor-pointer hover:text-[#1875D0] transition-colors">
          <i class="fa fa-envelope-o fa-2x"></i>
        </li>
        <li class="text-[#7B7B7B] cursor-pointer hover:text-[#1875D0] transition-colors">
          <i class="fa fa-share-alt fa-2x"></i>
        </li>
      </ul>

      <!-- Floating Action Button -->
      <div class="fab absolute right-[50px] bottom-[-40px] flex h-20 w-20 items-center justify-center rounded-full bg-[#1875D0] text-white shadow-[10px_10px_50px_0px_rgba(0,0,0,0.75)] cursor-pointer hover:bg-[#1565C0] transition-colors">
        <i class="fa fa-arrow-down fa-3x"></i>
      </div>
    </div>
  </div>
</template>

<script setup>
// اگر نیاز به لاجیک اضافی دارید، اینجا اضافه کنید
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css?family=Roboto');
@import url('https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css');

.font-roboto {
  font-family: 'Roboto', sans-serif;
}

/* Responsive adjustments */
@media (max-width: 968px) {
  .card {
    height: auto;
    width: 90%;
  }
  
  .thumbnail {
    position: relative;
    left: 0;
    top: 0;
    width: 100%;
    height: 250px;
    margin-bottom: 20px;
  }
  
  .right {
    margin-left: 20px;
    margin-right: 20px;
    padding-bottom: 100px;
  }
  
  h5, h6 {
    left: 20px;
  }
  
  ul {
    margin-left: 20px;
    bottom: 50px;
  }
  
  .fab {
    right: 20px;
  }
  
  .s1, .s2 {
    font-size: 8rem;
  }
}
</style>
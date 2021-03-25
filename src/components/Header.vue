<template>
    <div class="header">
      <div class="content">
        <span class="name">Matt Royer</span>
        <span class="title">PlayOn Coding Challenge</span>
      </div>
    </div>
</template>

<style lang="scss">
  .header {
      padding: 40px 30px;
      background-image: linear-gradient(180deg,#072641,#001c35);
      font-weight: bold;

      .content {
          text-align: center;
          max-width: 1100px;
          margin: 0 auto;
          
          @media screen and (min-width: 500px) {
              text-align: left;
          }
      }

      .name {
          font-size: 1.4em;
          color: #ffffff;
      }

      .name,
      .title {
          display: block;
      
          @media screen and (min-width: 500px) {
              display: inline-block;
          }
      }

      @media screen and (min-width: 500px) {
          .title {
              float: right;
              line-height: 2em;
          }
      }
  }
</style>

<script></script>

<div class="separateur">
  <div class="ligne"></div>
  <div class="ligne"></div>
  <div class="ligne"></div>
  <div class="ligne"></div>
  <div class="ligne"></div>
  <div class="ligne"></div>
  <div class="ligne"></div>
  <div class="ligne"></div>
  <div class="ligne"></div>
  <div class="ligne"></div>
</div>

<style>
  .separateur {
    width: 95%;
    height: 30px;
    display: flex;
  }
  .ligne {
    height: 100%;
    width: 100px;
    background-color: rgba(0, 0, 0, 0.831);
    margin: 0 10px;
    border-radius: 20px;
  }
  @media only screen and (max-width: 768px) {
    .separateur {
      width: 95%;
      height: 30px;
      display: flex;
      background-color: rgba(0, 0, 0, 0.831);
      border-radius: 20px;
    }
    .ligne {
      display: none;
    }
  }
</style>

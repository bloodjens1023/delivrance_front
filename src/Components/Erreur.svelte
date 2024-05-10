<script>
  var admin = false;
  var user = false;
  var chef = false;
  var aucun = false;
  try {
    let id = sessionStorage.getItem("identifiant");

    if (id == null || id == undefined || id == "") {
      console.log("null");
      aucun = true;
    } else {
      user = true;
    }
  } catch (error) {}
  try {
    let id = sessionStorage.getItem("chef");

    if (id == null || id == undefined || id == "") {
      console.log("null");
      aucun = true;
    } else {
      chef = true;
    }
  } catch (error) {}
</script>

<div class="center">
  <div class="error">
    <div class="number">4</div>
    <div class="illustration">
      <div class="circle"></div>
      <div class="clip">
        <div class="paper">
          <div class="face">
            <div class="eyes">
              <div class="eye eye-left"></div>
              <div class="eye eye-right"></div>
            </div>
            <div class="rosyCheeks rosyCheeks-left"></div>
            <div class="rosyCheeks rosyCheeks-right"></div>
            <div class="mouth"></div>
          </div>
        </div>
      </div>
    </div>
    <div class="numbers">0</div>
    <div class="number">4</div>
  </div>

  <div class="text">Oops. La page ou tu navige n'existe pas.</div>
  <br />
  <br />
  <a class="btn btn-dark" href="/">Retourner a l'accueil</a>
</div>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Reddit+Mono:wght@200..900&display=swap");
  * {
    font-family: "Poppins";
  }
  a {
    text-decoration: none;
  }

  .center {
    height: 100vh;
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
  }

  .error {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-content: center;
  }

  .number {
    font-weight: 900;
    font-size: 15rem;
    line-height: 1;
  }
  .numbers {
    display: none;
    font-weight: 900;
    font-size: 15rem;
    line-height: 1;
  }

  .illustration {
    position: relative;
    width: 12.2rem;
    margin: 0 2.1rem;
  }

  .circle {
    position: absolute;
    bottom: 0;
    left: 0;
    width: 12.2rem;
    height: 11.4rem;
    border-radius: 50%;
    background-color: #293b49;
  }

  .clip {
    position: absolute;
    bottom: 0.3rem;
    left: 50%;
    transform: translateX(-50%);
    overflow: hidden;
    width: 12.5rem;
    height: 13rem;
    border-radius: 0 0 50% 50%;
  }

  .paper {
    position: absolute;
    bottom: -0.3rem;
    left: 50%;
    transform: translateX(-50%);
    width: 9.2rem;
    height: 12.4rem;
    border: 0.3rem solid #293b49;
    background-color: white;
    border-radius: 0.8rem;

    &::before {
      content: "";
      position: absolute;
      top: -0.7rem;
      right: -0.7rem;
      width: 1.4rem;
      height: 1rem;
      background-color: white;
      border-bottom: 0.3rem solid #293b49;
      transform: rotate(45deg);
    }
  }

  .face {
    position: relative;
    margin-top: 2.3rem;
  }

  .eyes {
    position: absolute;
    top: 0;
    left: 2.4rem;
    width: 4.6rem;
    height: 0.8rem;
  }

  .eye {
    position: absolute;
    bottom: 0;
    width: 0.8rem;
    height: 0.8rem;
    border-radius: 50%;
    background-color: #293b49;
    animation-name: eye;
    animation-duration: 4s;
    animation-iteration-count: infinite;
    animation-timing-function: ease-in-out;
  }
  .eye-left {
    left: 0;
  }

  .eye-right {
    right: 0;
  }

  @keyframes eye {
    0% {
      height: 0.8rem;
    }
    50% {
      height: 0.8rem;
    }
    52% {
      height: 0.1rem;
    }
    54% {
      height: 0.8rem;
    }
    100% {
      height: 0.8rem;
    }
  }

  .rosyCheeks {
    position: absolute;
    top: 1.6rem;
    width: 1rem;
    height: 0.2rem;
    border-radius: 50%;
    background-color: #fdabaf;

    &-left {
      left: 1.4rem;
    }

    &-right {
      right: 1.4rem;
    }
  }

  .mouth {
    position: absolute;
    top: 3.1rem;
    left: 50%;
    width: 1.6rem;
    height: 0.2rem;
    border-radius: 0.1rem;
    transform: translateX(-50%);
    background-color: #293b49;
  }

  .text {
    margin-top: 5rem;
    font-size: 2rem;
    font-weight: bold;
    color: #293b49;
  }
  @media only screen and (max-width: 768px) {
    .text {
      margin-top: 2rem;
      font-size: 1.2rem;
      font-weight: bold;
      color: #293b49;
    }
    .number {
      font-weight: 900;
      font-size: 5rem;
      line-height: 1;
    }
    .numbers {
      display: block;
      font-weight: 900;
      font-size: 5rem;
      line-height: 1;
    }
    .illustration {
      display: none;
    }
  }
</style>

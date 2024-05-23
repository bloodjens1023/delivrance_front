<div class="checkmark"></div>

<style>
  .checkmark {
    width: 36px;
    height: 36px;
    display: inline-block;
    border-radius: 50%;
    background-color: #099f2a;
    position: relative;
  }

  .checkmark::after {
    content: "";
    position: absolute;
    left: 13px;
    top: 6px;
    width: 10px;
    height: 20px;
    border: solid white;
    border-width: 0 4px 4px 0;
    transform: rotate(45deg);
  }

  .checkmark {
    animation: fadeIn 0.5s ease-in-out forwards;
  }

  @keyframes fadeIn {
    from {
      opacity: 0;
      transform: scale(0);
    }
    to {
      opacity: 1;
      transform: scale(1);
    }
  }
</style>

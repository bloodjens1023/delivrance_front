<!-- UnChecker.svelte -->
<div class="unchecker"></div>

<style>
  .unchecker {
    position: relative;
    animation: 0.5s all;
    width: 36px;
    height: 36px;
    display: inline-block;
    border-radius: 50%;
    background-color: #d90000;
    position: relative;
  }

  .unchecker::before,
  .unchecker::after {
    content: "";
    position: absolute;
    width: 26px;
    height: 4px;
    background-color: #ffffff;
    top: 16px;
    left: 5px;
    opacity: 0;
  }

  .unchecker::before {
    transform: rotate(45deg);
  }

  .unchecker::after {
    transform: rotate(-45deg);
  }

  .unchecker::before,
  .unchecker::after {
    animation: drawUncheckmark 0.5s forwards 0.5s;
  }

  @keyframes spin {
    to {
      transform: rotate(360deg);
    }
  }

  @keyframes fadeIn {
    from {
      opacity: 0;
    }
    to {
      opacity: 1;
    }
  }

  @keyframes drawCheckmark {
    to {
      opacity: 1;
    }
  }

  @keyframes drawUncheckmark {
    to {
      opacity: 1;
    }
  }
</style>

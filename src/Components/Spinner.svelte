<div class="spinner"></div>

<!-- Spinner.svelte -->
<style>
  .spinner {
    border: 6px solid rgba(0, 0, 0, 0.1);
    border-left-color: #000000;
    border-radius: 50%;
    width: 36px;
    height: 36px;
    animation: spin 1s linear infinite;
  }

  @keyframes spin {
    to {
      transform: rotate(360deg);
    }
  }
  @media only screen and (max-width: 768px) {
    .spinner {
      width: 30px;
      height: 30px;
      border: 4px solid rgba(0, 0, 0, 0.1);
      border-left-color: #000000;
    }
  }
</style>

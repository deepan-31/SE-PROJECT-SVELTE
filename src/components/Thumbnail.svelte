<script>
    export let image;
  
    // Assuming `image` is an object with properties `url` and `alt`
    const { url, alt } = image;
  </script>
  
  <img src={url} alt={alt} />
  
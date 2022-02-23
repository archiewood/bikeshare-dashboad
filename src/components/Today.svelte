<script>
    export let dateColor = 'green'; 
    var today = new Date().toLocaleDateString();
</script>

<span style="color:{dateColor};">{today}</span>
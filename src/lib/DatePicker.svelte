<script lang="ts">
  export let startDate = new Date();
  export let endDate = new Date();
  let isOpen = false;

  const onClearDates = () => {
    startDate = new Date();
    endDate = new Date();
  };

  const toggleDatePicker = () => (isOpen = !isOpen);
</script>

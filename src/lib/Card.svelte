<script>
    export let title = "default title";
    export let timeline = "default timeline";
</script>

<style>
    .card {
       border: 1px solid #ddd;
       border-radius: 8px;
       padding: 1.5rem;
       margin: 1rem 0;
       box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
       background-color: white;
    }
 
    .card-header {
       display: flex;               /* Flexbox für Titel und Timeline */
       justify-content: space-between;  /* Abstand zwischen Titel und Timeline */
       align-items: center;          /* Beide Elemente vertikal zentrieren */
       margin-bottom: 1rem;
    }
 
    .card h3 {
       margin: 0;
       color: var(--primary-color);
       font-size: 1.25rem;
    }
 
    .timeline {
       font-size: 0.9rem;             /* Kleinere Schriftgröße */
       font-weight: bold;             /* Fettgedruckt */
       color: var(--secondary-color);
    }
 
    .card .content {
       color: var(--text-color);
    }

   /* Hover effect */
   .card:hover {
       box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2); /* More pronounced shadow on hover */
       transform: translateY(-5px); /* Slight upward movement */
   }
   
   /* Optionally, change background on hover */
   .card:hover {
       background-color: rgba(255, 255, 255, 0.9);
   }
 </style>
 
 <div class="card" >
    <div class="card-header">
        <h3>{title}</h3>
        <strong class="timeline">{timeline}</strong>
    </div>
    <div class="content">
        <slot></slot>
     </div>
 </div>
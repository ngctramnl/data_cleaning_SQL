# Cleaning Data in SQL
- The changelog can be found in the notebook uploaded. The notebook was downloaded from a [workspace](https://app.datacamp.com/workspace/w/b0bcb8ab-dc5b-45f8-a660-5c52890c5bec/edit) on Datacamp.
  
```-- Identifying missing data --
SELECT COUNT(*) AS number_missing_unemployment_rates
FROM world.economies
WHERE unemployment_rate IS NULL;```

# Visualizing findings on Tableau
<div class='tableauPlaceholder' id='viz1713451852391' style='position: relative'><noscript><a href='#'><img alt='Dashboard 1 ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Ec&#47;Economies_17134501777760&#47;Dashboard1&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='site_root' value='' /><param name='name' value='Economies_17134501777760&#47;Dashboard1' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Ec&#47;Economies_17134501777760&#47;Dashboard1&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='language' value='en-GB' /></object></div>                


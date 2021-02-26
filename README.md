# sf-old-street-names

## Fun with feature name substitution using tangram.js

- some [old San Francisco street names](https://burritojustice.github.io/sf-old-street-names/) in red

- also [Ferlinghetti Ave](https://burritojustice.github.io/sf-old-street-names/ferlinghetti.html)

## How does it work?

In the scene file, you can create a yaml list of streets and substitutions.

  
    streets_1861:
        Divisadero St.: Devisadaro St.
        Ocean Ave.: Ocean House Rd.
        San Jose Ave.: Old San Jose Rd.
        
        
You can also reference them by OSM ID:
  
    id_1861:    
        8921644: El Dorado St.
        391862870: Santa Clara St.
        88572914: Plank Road
        88572941: Plank Road
        88572932: Plank Road
        51392402: Plank Road
        51392400: Plank Road
        
Then you define a global function that looks for those names or IDs, and substitutes in what you want:

      swap_text_source: | 
        function() {
            return global.id_1861[feature.id] || global.streets_1861[feature.name] || feature.name;
            }
            
You could put that function in this one, but this works and I'm not messing with it right now:

    draw_swap_txt_source:
        draw:
            text-blend-order:
                text_source: global.swap_text_source

The trickier part is finding the road types in whatever map style you're trying to hijack -- in this case, it's [Refill](https://github.com/tangrams/refill-style). (This is laborious and you need a pretty good understanding of how road labels work, but you can see what's happening.)

  layers:
    pois:
        highway-exit:
            visible: false
    roads: 
        major_road:
            trunk_primary:
                routes:
                    labels-trunk_primary-route-z16:
                        old_labels: global.draw_swap_txt_source
                    labels-trunk_primary-route-z17-z18:
                        old_labels: global.draw_swap_txt_source
                labels-trunk_primary-z13:
                    old_labels: global.draw_swap_txt_source
                labels-trunk_primary-z14:
                    old_labels: global.draw_swap_txt_source
                labels-trunk_primary-z15:
                    old_labels: global.draw_swap_txt_source
                labels-trunk_primary-z16:
                    old_labels: global.draw_swap_txt_source
                labels-trunk_primary-z17:
                    old_labels: global.draw_swap_txt_source
                labels-trunk_primary-z18:
                    old_labels: global.draw_swap_txt_source
            secondary:
                labels-secondary-z15:
                    old_labels: global.draw_swap_txt_source
                labels-secondary-z16:

## To Do

- add a keypress event to toggle the names back and forth for a `map time machine`

---
layout: default
title: stroomverbuik
has_children: false
nav_order: 2
---

# stroomverbuik

| stroomverbuik per week         	|                         	|                       	|        	|               	|
|--------------------------------	|-------------------------	|-----------------------	|--------	|---------------	|
|                                	|                         	| µA gemiddeld verbuik  	|        	| Wh            	|
| gas sensor BME680              	| ULP                     	| 80                    	|        	| 0.044352      	|
| voeding                        	|                         	| 0.8                   	|        	| 0.00044352    	|
| battery level (spanningsdeler) 	|                         	| 1                     	|        	| 0.0005544     	|
|                                	|                         	|                       	|        	|               	|
| beweging sensor [worst case]   	|                         	|                       	|        	|               	|
| beweging gedecteerd [uur]      	| 60                      	|                       	|        	|               	|
| tijd [uur]                     	| mode                    	| I[µA]                 	|        	|               	|
| 60.00                          	| actief                  	| 8                     	|        	| 0.001584      	|
| beweging niet gedecteerd [uur] 	| 108                     	|                       	|        	|               	|
| tijd [uur]                     	| mode                    	| I[µA]                 	|        	|               	|
| 108.00                         	| standby                 	| 6                     	|        	| 0.0021384     	|
|                                	|                         	|                       	|        	|               	|
| esp32 schatting                	| aantal metingen / uur   	| 12                    	|        	|               	|
|                                	| max waarde overschreden 	| 15                    	|        	|               	|
| tijd [s]                       	| mode                    	| I[mA]                 	| aantal 	|               	|
| 5.00                           	| wifi                    	| 130                   	| 183    	| 0.1090375     	|
| 597837                         	| sleep                   	| 0.005                 	| 1      	| 0.00274008625 	|
| 3                              	| actief                  	| 20                    	| 2016   	| 0.11088       	|
|                                	|                         	|                       	|        	|               	|
| buzzer [s]                     	| 1                       	| 30                    	| 15     	| 0.0004125     	|
|                                	|                         	|                       	|        	|               	|
|                                	|                         	|                       	|        	|               	|
|                                	| per week                	| 0.2721424063          	| Wh     	|               	|
|                                	|                         	|                       	|        	|               	|
|                                	| per jaar                	| 14.15140513           	| Wh     	|               	|

Een AA baterij heeft een gemiddelde capaciteit van 2-3 Wh, dus indien we een jaar willen doorkomen hebben we nood aan 5 batterijen.

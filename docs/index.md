---
layout: default
title: stroomverbuik
has_children: false
nav_order: 2
---

# stroomverbuik

|      stroomverbuik per week     	|                       	|                       	|        	|               	|
|-----------------------------------|:------------------------|-------------------------|---------|-----------------|
| gas sensor BME680               	|                       	| µA gemiddeld verbuik  	|        	|               	|
|                                 	| ULP                   	|                    80 	|        	|      0.044352 	|
| voeding                         	|                       	|                   0.8 	|        	|    0.00044352 	|
| battery level (spanningsdeleer) 	|                       	|                     1 	|        	|     0.0005544 	|
|                                 	|                       	|                       	|        	|               	|
| beweging sensor [worst case]    	|                       	|                       	|        	|               	|
| beweging gedecteerd [uur]       	|                    60 	|                       	|        	|               	|
| tijd [uur]                      	| mode                  	| I[µA]                 	|        	| Wh            	|
|                           60.00 	| actief                	|                   106 	|        	|      0.020988 	|
| beweging niet gedecteerd [uur]  	|                   108 	|                       	|        	|               	|
| tijd [uur]                      	| mode                  	| I[µA]                 	|        	| Wh            	|
|                          108.00 	| standby               	|                     6 	|        	|     0.0021384 	|
|                                 	|                       	|                       	|        	|               	|
| esp32 schatting                 	| aantal metingen / uur 	|                    12 	|        	|               	|
| tijd [s]                        	| mode                  	| I[mA]                 	| aantal 	| Wh            	|
|                            5.00 	| wifi                  	|                   130 	|    183 	|     0.1090375 	|
|                          603885 	| sleep                 	|                 0.005 	|      1 	| 0.00276780625 	|
|                               5 	| actief                	|                    20 	|    243 	|      0.022275 	|
|                                 	|                       	|                       	|        	|               	|
|                                 	|                       	|                       	|        	|               	|
|                                 	|                       	|                       	|        	|               	|
|                                 	|                       	|                       	|        	|               	|
|                                 	| per week              	|          0.2025566263 	| Wh     	|               	|
|                                 	|                       	|                       	|        	|               	|
|                                 	| per jaar              	|           10.53294457 	| Wh     	|               	|

Een AA baterij heeft een gemiddelde capaciteit van 2-3 Wh, dus indien we een jaar willen doorkomen hebben we nood aan 5 batterijen.

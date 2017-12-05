**Metric algorithms summary**


| Name      	| Avg exec time 	| mistakes, elements     	| train set                   	|
|-----------	|---------------	|------------------------	|-----------------------------	|
| kNN       	| ~35s          	| 5                      	| iris                        	|
| WkNN      	| ~45s          	| 6                      	| iris                        	|
| PW        	| ~50s          	| 6                      	| iris                        	|
| PF        	| *unstable*    	| user-defined threshold 	| iris                        	|
| STOLP-kNN 	| ~70s          	| user-defined threshold 	| compressed iris (150 => 16) 	|

# Switch IP Sheet
#northcoast #network #docs 

## Vista Switches

| Building        | Name                | IP                   | Members | Model           |
|-----------------|---------------------|----------------------|---------|-----------------|
| MDF             | MDF Core Switch     | [172.20.1.1](ssh://nccadmin@172.20.1.1)             | 5       | 6248P           |
| Womens Restroom | Bldg1A-Restroom     | [172.20.101.3](ssh://nccadmin@172.20.101.3 )  | 1       | 6224            |
| Master Control  | MC Core Switch      | [172.20.111.1](ssh://nccadmin@172.20.111.1)         | 2       | 6248P           |
| Master Control  | MC Access SW        | `ssh nccadmin@172.20.111.2 -oKexAlgorithms=+diffie-hellman-group1-sha1 -c 3des-cbc`| 1       | 3524P           |
| Bldg 2          | Bldg 2 Access       | [172.20.103.10](ssh://nccadmin@172.20.103.10)       | 1       | 6224            |
| Bldg 3          | Bldg 3 Core Stack   | [172.20.103.1](ssh://nccadmin@172.20.103.1)         | 2       | 6248P           |
| Bldg 3          | Bldg 3 Access       | `ssh nccadmin@172.20.103.2 -oKexAlgorithms=+diffie-hellman-group1-sha1 -c 3des-cbc`| 1       | 3524P           |
| Bldg 3          | Bldg 3 Sec Access A | [172.20.103.3](ssh://nccadmin@172.20.103.3)         | 1       | 6248P           |
| Bldg 3          | Bldg 3 Sec Access B | [172.20.103.4](ssh://nccadmin@172.20.103.4)         | 1       | 6248P           |
| Bldg 4          | Bldg 4 Core Stack   | [172.20.104.1](ssh://nccadmin@172.20.104.1)         | 6       | 2- 6248/4-6248P |
| Bldg 4          | Bldg 4 Access A     | `ssh nccadmin@172.20.104.2 -oKexAlgorithms=+diffie-hellman-group1-sha1 -c 3des-cbc`| 1       | 3548P           |
| Bldg 4          | Bldg 4 Access B     | `ssh nccadmin@172.20.104.3 -oKexAlgorithms=+diffie-hellman-group1-sha1 -c 3des-cbc`| 1       | 3548P           |
| Bldg 4          | Bldg 4 Access D     | `ssh nccadmin@172.20.104.5 -oKexAlgorithms=+diffie-hellman-group1-sha1 -c 3des-cbc`| 1       | 35248           |
| Bldg 4          | Bldg 4 Sec Access A | [172.20.104.8](ssh://nccadmin@172.20.104.8)         | 1       | 6248P           |
| Bldg 4          | Bldg 4 Sec Access B | [172.20.104.9](ssh://nccadmin@172.20.104.9)         | 1       | 6248P           |
| Bldg 5          | Bldg 5 Core Stack   | [172.20.105.1](ssh://nccadmin@172.20.105.1)         | 3       | 6248P           |
| Bldg 5          | Bldg 5 Access A     | `ssh nccadmin@172.20.105.2 -oKexAlgorithms=+diffie-hellman-group1-sha1 -c 3des-cbc`| 1       | 3548P           |
| Bldg 5          | Bldg 5 Sec Access A | [172.20.105.5](ssh://nccadmin@172.20.105.5)         | 1       | 6224p           |
| Bldg 6          | Bldg 6 Core Stack   | [172.20.162.1](ssh://nccadmin@172.20.162.1)         | 2       | 6248p           |
| Nine Ten        | Nine Ten Access     | [172.20.105.3](ssh://nccadmin@172.20.105.3)         | 1       | 6224U           |
| Eleven Twelve   | Eleven Twelve Access| [172.20.105.4](ssh://nccadmin@172.20.105.4)         | 1       | 6224U           |
| IT              | NON IT Switch No SSH?      | [172.20.111.6](ssh://nccadmin@172.20.111.6)  | 1       | 6248P           |
| IT              | IT Office Core      | [172.20.106.1](ssh://nccadmin@172.20.106.1)         | 2       | 6248P           |
| SEC             | Sec Core Switch     | [172.20.164.1](ssh://nccadmin@172.20.164.1)         | 1       | 6248P           |

## Fallbrook Switches
| Name                   | IP           | Members | Model        |
|------------------------|--------------|---------|--------------|
| Fallbrook Core Stack   | [172.21.1.1](ssh://nccadmin@172.21.1.1)     | 3       | 6248P        |
| Fallbrook Sec Access A | [172.21.101.2](ssh://nccadmin@172.21.101.2) | 1       | 6248P        |
| Fallbrook Sec Access B | [172.21.101.3](ssh://nccadmin@172.21.101.2) | 1       | 6248P        |

## Carlsbad Switches
| Name                   | IP           | Members | Model        |
|------------------------|--------------|---------|--------------|
| Cbad-CoreStack         | [172.22.1.1](ssh://nccadmin@172.22.1.1)   | 2       | 6248P        |

## Rancho Bernardo Switches
| Name                   | IP           | Members | Model        |
|------------------------|--------------|---------|--------------|
| SME-CoreStack          | [172.23.1.1](ssh://nccadmin@172.23.1.1)   | 2       | 6248P        |
| SME Access West        | [172.23.1.4](ssh://nccadmin@172.23.1.4)   | 2       | 6248U        |
| SME Access East        | [172.23.10.7](ssh://nccadmin@172.23.10.7) | 1       | 6224U        |

## Ramona Switches
| Name                   | IP           | Members | Model        |
|------------------------|--------------|---------|--------------|
| Ramona Core Stack      | [172.24.1.1](ssh://nccadmin@172.24.1.1)   | 2       | 6248P        |
| Bldg 1 SW              | 172.24.11.1  | 1       | X1000 Series |
| Bldg 3 SW              | 172.24.30.1  | 1       | X1000 Series |
| Bldg 4 SW              | 172.24.40.1  | 1       | X1000 Series |
| Bldg 5 SW              | 172.24.50.1  | 1       | X1000 Series |

## Rancho Bernardo Switches
| Name                   | IP           | Members | Model        |
|------------------------|--------------|---------|--------------|
| RB Core Stack          | [172.25.1.1](ssh://nccadmin@172.25.1.1)   | 4       | 6248P        |

## Pauma Valley Switches
| Name                   | IP           | Members | Model        |
|------------------------|--------------|---------|--------------|
| PV Core Stack          | [172.26.101.1](ssh://nccadmin@172.26.101.1)   | 1       | 6248P        |
| PV Marylans Office     | [172.26.101.2](ssh://nccadmin@172.26.101.2)   | 0       | 3650-CX      |
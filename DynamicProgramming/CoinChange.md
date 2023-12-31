## Solution



| Coin   | Current Amount | Old Value    | New Value    | Updated dp|
|--------|-----------------|--------------|--------------| |
|   1    |        1       | 2147483646= Integer.MAX_VALUE   |       1      |  |
|   1    |        2       | Integer.MAX_VALUE  |       2      |  |
|   1    |        3       | Integer.MAX_VALUE   |       3      |  |
|   1    |        4        | Integer.MAX_VALUE   |       4      | [0, 1, 2, 3, 4] |
|   2    |        2       | 2   |       1      |  |
|   2    |        3       | 3   |       2      |   |
|   2    |        4        | 4  |       2      | [0, 1, 1, 2, 2]   |
|   3    |        4       | 2  |       1      |   |
|   3    |        4       | 2  |       2      | [0, 1, 1, 1, 2] |


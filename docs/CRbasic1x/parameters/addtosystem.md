# AddToSystem

Determines how the newly created menu will be displayed in the datalogger's menuing system:

| Option | Description                                                                                                                                                           |
| ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0      | Upon power up, a starting window is displayed. When a key is pressed, the system menu will be displayed. The custom menu will appear as a submenu to the system menu  |
| -1     | Upon power up, a starting window is displayed. When a key is pressed, the custom menu will be displayed. The system menu will appear as a submenu to the custom menu. |
| -2     | Upon power up, the custom menu is displayed. The system menu will appear as a submenu to the custom menu.                                                             |
| -3     | Upon power up, the custom menu is displayed. The system menu is hidden from the user.                                                                                 |
| -4     | Upon power up, the custom menu is displayed. All of the system menu, except for Display Settings, is hidden from the user.                                            |

Type: Constant

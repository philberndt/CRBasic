# Manually triggering a VW diagnostic report **Follow these steps to trigger writing a diagnostic sensor file manually:** 1. OpenTableMonitorinLoggerNetand locate the name specified for the DiagFName parameter in the Public table.

2. Using theTable Monitor, enter the desired file name for the diagnostic data file. This specifies the target file for writing. Be sure to include the directory name when entering the file name. For example, CPU:FileName. Valid directories include CPU:, CRD:, USR:,. Do not include the .vw extension; it will be appended to file name automatically.

   Entering a file name into the variable specified for DiagFName triggers the data logger to write the diagnostic file to the specified drive. After the file is written, the DiagFName field is cleared and can be triggered again by entering a new file name.

The image below demonstrates this procedure with the default DiagFName (**\_VW_VWFile **). In this example, a diagnostic file nameddiag.vwwill be written to the CPU. Once the file is successfully written, the**\_VW_VWFile** field is cleared and is ready to trigger again by entering a new file name.

The resulting diagnostic file can be retrieved either throughFile Controlor by removing the storage card. The file can then be viewed using theDevice Configuration UtilityinVW Offline Analysismode.

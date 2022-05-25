## Container

### Frontend
|| Name | Image | Version | Expose | Database |
| :-: | - | - | :-: | :-: | :-: |
| 1 | connector | 139.5.146.7:5000/csp.etaxsp.connector.api | 1.1.0 | 55001 | :x: |

---

### Backend

|| Name | Image | Version | Expose | Database |
| :-: | - | - | :-: | :-: | :-: |
| 1 | [receive](#receive) | 139.5.146.7:5000/csp.etaxsp.receive.api | 1.1.6 | 55006 | :heavy_check_mark: |
| 2 | [system](#system) | 139.5.146.7:5000/csp.etaxsp.system.api | 1.1.2 | 55008 | :heavy_check_mark: |
| 3 | [common](#common) | 139.5.146.7:5000/csp.etaxsp.common.api | 1.1.3 | 55004 | :heavy_check_mark: |
| 4 | [core](#core) | 139.5.146.7:5000/csp.etaxsp.core.service | 1.3.5 | 55000 | :heavy_check_mark: |
| 5 | [process](#process) | 139.5.146.7:5000/csp.etaxsp.process.api | 1.1.9 | 55002 | :heavy_check_mark: |
| 6 | [configuration](#configuration) | 139.5.146.7:5000/csp.etaxsp.configuration.api | 1.1.0 | 55003 | :heavy_check_mark: |
| 7 | [generatepdf](#generatepdf) | 139.5.146.7:5000/csp.etaxsp.generatepdf.api | 1.1.3 | 55013 | :heavy_check_mark: |
| 8 | [generatexml](#generatexml) | 139.5.146.7:5000/csp.etaxsp.generatexml.api | 1.1.1 | 55010 | :heavy_check_mark: |
| 9 | [transferxml](#transferxml) | 139.5.146.7:5000/csp.etaxsp.transferxml.api | 1.1.2 | 55009 | :heavy_check_mark: |
| 10 | [transferxml-service](#transferxml-service) | 139.5.146.7:5000/csp.etaxsp.transferxml.service | 1.1.4 | 55015 | :heavy_check_mark: |
| 11 | [shorturl](#shorturl) | 139.5.146.7:5000/csp.etaxsp.convertshorturl.api | 1.1.0 | 55012 | :heavy_check_mark: |
| 12 | [validatexml](#validatexml) | 139.5.146.7:5000/csp.etaxsp.validatexml.api | 1.1.0 | 55011 | :heavy_check_mark: |
| 13 | [log](#log) | 139.5.146.7:5000/csp.etaxsp.senddocument.api | 1.1.0 | 55014 | :heavy_check_mark: |
| 14 | [senddoc](#senddoc) | 139.5.146.7:5000/csp.etaxsp.receive.api | 1.1.6 | 55002 | :heavy_check_mark: |
| 14 | [senddoc-tracking](#senddoc-tracking) | 139.5.146.7:5000/csp.etaxsp.senddocument.tracking.service | 1.1.1 | 55016 | :heavy_check_mark: |
| 15 | [notification](#notification) | 139.5.146.7:5000/csp.etaxsp.notification.service | 1.1.6 | 55019 | :heavy_check_mark: |
| 16 | [report](#report) | 139.5.146.7:5000/csp.etaxsp.report.api | 1.1.1 | 55018 | :heavy_check_mark: |
| 17 | [recovery](#recovery) | 139.5.146.7:5000/csp.etaxsp.recovery.api | 1.0.1 | 55017 | :heavy_check_mark: |

---

### Service

|| Name | Image | Version | Expose | Database |
| :-: | - | - | :-: | :-: | :-: |
| 1 | gmc | 139.5.146.7:5000/inspire.productionserver | latest | 30354 | :x: |

---

#### Receive 

##### **Container Ports**
| Expose | Target |
| :-: | :-: |
| 80 | 55002 |

##### **Container Volume**
| Mount Path | Container Path |
| - | - |
| ./config/receive/ | /app/Configuration |
| ./logs/receive/ | /app/logs |

##### **Database Config**
| Server | Database | User |
| - | - | - |
| 10.148.1.17,33411 | eTaxSP_Receive | sreceive |

##### **System Link Config**
|| System |
| :-: | - |
| 1 | system |
| 2 | configuration |

##### Checker
| Config |  |
| - | - |
| HealthStatus | :heavy_check_mark: |
| nlog | :heavy_check_mark: |

[top](#receive)

---

#### System


[top](#system)

---

#### Common


[top](#common)

---

#### Core


[top](#core)

---

#### Process


[top](#process)

---

#### Configuration


[top](#configuration)

---

#### GeneratePDF


[top](#generatepdf)

---

#### GenerateXML


[top](#generatexml)

---

#### TransferXML


[top](#transferxml)

---

#### TransferXML-Service
 

[top](#transferxml-service)

---

#### Shorturl
 

[top](#shorturl)

---

#### Database Configuration

|| Name | Database | User |
| :-: | - | - | - |
| 1 | receive | eTaxSP_Receive | sreceive |
| 1 | receive | eTaxSP_Account | saccount |
| 1 | receive | eTaxSP_ArchiveSystem | sarchive |
| 1 | receive | eTaxSP_Common | scommon |
| 1 | receive | eTaxSP_Corporate_{0}_Configuration | sconfiguration |
| 1 | receive | eTaxSP_Corporate_{0}_Process | sprocess |
| 1 | receive | eTaxSP_Corporate_{0}_Log | slog |
| 1 | receive | eTaxSP_Corporate_{0}_SendDocument | ssenddocument |
| 1 | receive | eTaxSP_Corporate_{0}_TransferXML | stransferxml |
| 1 | receive | eTaxSP_System | ssystem |


## Modules

Both server and client extensions are packaged as _modules_ which are optionally loaded in a _database_. A module is a collection of functions and data that target a single purpose.

### Composition of a module[](https://www.odoo.com/documentation/20.0/developer/tutorials/server_framework_101/01_architecture.html#composition-of-a-module "Permalink to this headline")

An Odoo module **can** contain a number of elements:

[Business objects](https://www.odoo.com/documentation/20.0/developer/reference/backend/orm.html#reference-orm)

A business object (e.g. an invoice) is declared as a Python class. The fields defined in these classes are automatically mapped to database columns thanks to the ORM layer.

[Object views](https://www.odoo.com/documentation/20.0/developer/reference/user_interface/view_architectures.html)

Define UI display

[Data files](https://www.odoo.com/documentation/20.0/developer/reference/backend/data.html#reference-data)

XML or CSV files declaring the model data:

- [views](https://www.odoo.com/documentation/20.0/developer/reference/user_interface/view_architectures.html) or [reports](https://www.odoo.com/documentation/20.0/developer/reference/backend/reports.html#reference-reports),
    
- configuration data (modules parametrization, [security rules](https://www.odoo.com/documentation/20.0/developer/reference/backend/security.html#reference-security)),
    
- demonstration data
    
- and more
    

[Web controllers](https://www.odoo.com/documentation/20.0/developer/reference/backend/http.html#reference-controllers)

Handle requests from web browsers



### Manifest
Il file manifest è chiamato: `__manifest__.py`
Contiene un dict, in ogni field di questo dict ogni chiave specifica un metadato del modulo.

La parte più significativa specificata nel manifest sono le sue dependencies, sia di altri odoo modules, sia di librerie python relative ad esso.

https://www.odoo.com/documentation/20.0/developer/reference/backend/module.html#manifest

## Domande

### Developer mode

Funzioni sbloccate: Update app list per ricaricare la lista moduli a runtime.




### Verifica
3. La cartella in cui odoo cerca i custom modules.
4. Perchè sono cartelle che vanno configurate dall'utente, richiederebbero di rigenerare layer ad ogni cambiamento. 
5. le modifiche ai pyhon files richiedono un riavvio del servizio. -d per specificare il database -u per specificare il modulo da upgradare. 
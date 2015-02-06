
![Build Status](https://img.shields.io/badge/branch-master-blue.svg) [![Build Status](https://travis-ci.org/m1ome/phalcon-datatables.svg?branch=master)](https://travis-ci.org/m1ome/phalcon-datatables)

# About
This is a [Phalcon Framework](http://phalconphp.com/) adapter for [DataTables](http://www.datatables.net/).
# Support
### Currently supported
* QueryBuilder interface
* ResultSet interface
* Pagination
* Global search (by value)
* Ordering
* Multiple column ordering
* Column-based search

# Installation
### Installation via Composer
* Install a composer
* Create `composer.json` file inside your project directory
* Paste into it
```json
{
    "require": {
        "m1ome/phalcon-datatables": "dev-master"
    }
}
```
* Run `composer install`

# Example usage
It uses Phalcon [QueryBuilder](http://docs.phalconphp.com/en/latest/api/Phalcon_Mvc_Model_Query_Builder.html) for pagination in DataTables.

In example we have a stantart MVC application, with database enabled. Don't need to provide a normal bootstrap PHP file, for Phalcon documentation, visit official site.

### Controller (using QueryBuilder):
```php
<?php
use \DataTables\DataTable;

class TestController extends \Phalcon\Mvc\Controller {
    public function indexAction() {
        if ($this->request->isAjax()) {
          $builder = $this->modelsManager->createBuilder()
                          ->columns('id, name, email, balance')
                          ->from('Example\Models\User');

          $dataTables = new DataTable();
          $dataTables->fromBuilder($builder)->sendResponse();
        }
    }
}
```

### Controller (using ResultSet):
```php
<?php
use \DataTables\DataTable;

class TestController extends \Phalcon\Mvc\Controller {
    public function indexAction() {
        if ($this->request->isAjax()) {
          $resultset  = $this->modelsManager->createQuery("SELECT * FROM \Example\Models\User")
                             ->execute();

          $dataTables = new DataTable();
          $dataTables->fromResultSet($resultset)->sendResponse();
        }
    }
}
```

### Model:
```php
<?php
/**
* @property integer id
* @property string name
* @property string email
* @property float balance
*/
class User extends \Phalcon\Mvc\Model {
}
```

### View:
```html
<html>
    <head>
        <title>Simple DataTables Application</title>
        <script type="text/javascript" language="javascript" src="//code.jquery.com/jquery-1.11.1.min.js"></script>
        <script type="text/javascript" language="javascript" src="//cdn.datatables.net/1.10.4/js/jquery.dataTables.min.js"></script>
        <script type="text/javascript">
            $(document).ready(function() {
                $('#example').DataTable({
                    serverSide: true,
                    ajax: {
                        url: '/test/index',
                        method: 'POST'
                    },
                    columns: {
                        {data: "id", searchable: false},
                        {data: "name"},
                        {data: "email"},
                        {data: "balance", searchable: false}
                    }
                });
            });
        </script>
    </head>
    <body>
        <table id="example">
            <thead>
                <th>ID</th>
                <th>Username</th>
                <th>Email</th>
                <th>Balance</th>
            </thead>
            <tbody>
            </tbody>
        </table>
    </body>
</html>
```

# More examples
For more examples please search in `site` directory.
It contains basic *Phalcon* bootstrap page to show all Phalcon-DataTables functionality.

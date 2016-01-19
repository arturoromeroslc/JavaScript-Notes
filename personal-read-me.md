# Facet Search
>
/home/arturo/Projects/rpmsolutions/app/views/_brain/themes/tsn/scripts/catalog/tires.ctp
/home/arturo/Projects/rpmsolutions/app/views/_brain/themes/tsn/scripts/catalog/search.ctp
/home/arturo/Projects/rpmsolutions/app/views/_shared/scripts/catalog/search.ctp
/home/arturo/Projects/rpmsolutions/app/controllers/catalog_controller.php
/home/arturo/Projects/rpmsolutions/app/views/_brain/themes/tsn/elements/catalog/filters.ctp

> The issue is that when you select the checkbox for brand, the code doesn't filter out the other brands. 


> onClick of a checkbox = reloadSearchResults is called and then the tires.ctp.displayResult(tires) is called with the $products php variable as an argument in order to show tires.

> The enableFilters function is called whenever a checkbox is selected. 

> I need to find out before the reloadSearchResults function is called what php is doing to get the brands back which then passed to filterFilters array as $filter_Id.

> The query on catalog_controller.php for search is returning all of the brand tires.
> The issue is that this is only happening when a user selects the brands checkbox.

> Inside of enable filters is where some of the filters get the disabled set to false.  Inside of .each(filters, function(filter, filtervals))


```js
$filter_ids


$.each(filters, function(filter, filtervals){
  debugger;
  $.each(filtervals, function(){ debugger;
    $('#' + this['id']).attr('disabled', false);
  });
});
```



> Then filterFilters(arr) is then called with the variable $filter_id.  Inside of $filter_id array is where all of the products for the filter data are stored. 

## Faceted Search
>
__findProducts is called with the filter object.
It collect the correct promtion id from $data['Promotion']

Conditions for the promotion was being set inside of _findProct this function was taking the filter object and calling getRebateLines(id=array) with the array of id
on the admin/model/promotions.  

Solution for returning multiple promotions was in adding the 'all' count to the find model method, it use to be 'first'

```
$lines = $this->find('all', array(
      'conditions' => array(
        'Promotion.id' => $promotion_id,
        'CURDATE() BETWEEN Promotion.begins AND Promotion.ends', // centralize
      ),
    ));
```

### Faceted 
> _findProducts inside of ```controller/catalog_controller.php``` is where the initial query for the products is.

detects a change in tire.ctp for the filter
the sets url paramas in ```addSearchFilters()``` tire.ctp
Then pushChangeUrl with Jquery with sets up hashchange event handler ```tire.ctp```
```haschange``` set all of the filter checkboxes to checked, false.
from haschange there is an AJAX call to _findProducts with the searchFilter obj as the data. 
the warranty needs to a tilda with an array on it similar to promotions and brand. 


## DEBUG CODE FOR LOCAL TIRE FINDER
>
found in views/_shared/scripts/app.ctp
```js
url: debug ? "http://wpapi.local"+apiRoute : "http://wpapi.tcsgeeks.com"+apiRoute

```
>
NOTE: added a class to the filters in the view did not work. 
function doSearch on the tires.ctp does an ajax call to the server with the filters selected, The callback functions on the load function get called with the filter_ID's param. 
The issue is that on the search function inside of the catalog_controller the current_conditions for brand has the SQL Query for manufacture id or array with width and ratio and tire.
The $data = $this->MyProduct->MyManufacturer->getRecordsWithActiveProducts($this->_current_conditions, false, 'MyProduct.manufacturer_id') calls:
```
  function getRecordsWithActiveProducts($conditions=array(), $list_only=true, $exclude=null){
        if (!empty($conditions) && $exclude) {
            if (isset($conditions['AND'])) {
                foreach ($conditions['AND'] AS $k => &$filter_type) {
                    if (is_array($filter_type) && key($filter_type)===$exclude) {
                        unset($conditions['AND'][0]);
                        break;
                    }
                }

                if (empty($conditions['AND'])) unset($conditions['AND']);
            }
        }

        if (isset($GLOBALS['Catalog']['use_installer_conditions']) && $GLOBALS['Catalog']['use_installer_conditions']==1) {
            return $this->getCatalogInstallerList($conditions, $list_only);
        }else{
            return $this->getCatalogList($conditions, $list_only);
        }
  }
```
> ###Old Code for warranties
```

$(".warranty-filter").bind('change', function(e) {
    
    if (enableFilters && !selected && !warranties) {
        var selected    = [],
            warranties  = [];
    
            
        $(".warranty").find("input[type=checkbox]:checked").each(function(index, element) {
            warranties.push($(element).attr('name').replace('-', '').replace(/k/g, '').split('  '));
            $.each(warranties, function(key, warranty){
                for (var warrantyItem = 0; warrantyItem < warranty.length; warrantyItem += 1) { debugger
                    if(warranty[warrantyItem] == 'No Warranty'){
                        warranty[warrantyItem] = 0;
                    }
                    warranty[warrantyItem] = parseInt(warranty[warrantyItem]);
                }
                if(typeof selected['max'] == 'undefined' || warranty > selected['max']){
                    selected['max'] = warranty;
                } 
                if(typeof selected['min'] == 'undefined' || warranty < selected['min']){
                    selected['min'] = warranty;
                }
            });
        });
        <? if (check_setting('Catalog', 'advanced_filters', 1)) { ?>
            $('#selected-warranty-id-'+$(this).attr("name")).show();
            // $(this).parent().hide();
        <? } ?>
        if(selected['max'] == 0){
            selected['max'] = 1;
        }
        searchFilter.WarrantyMax = selected['max'];
        searchFilter.WarrantyMin = selected['min'];
        addSearchFilters();
    }
});
```
>
Inside of MyManufactuer model.
If the $exclude === inArray($condition) {Remove $condition[$exclude]} 


## NEW CASE FOR MYINSTALLERS
>
The element for footer was kept the same and the new connct with us has the installer element checks
```
<? if (arr[i]) { 
    arr[i] 
    } else if (arr[i + 1]) { 
            arr[i +1] 
    }  ?>
```
## NEW CASE FOR BEAST-PHASE-5
>
The data is set on brain themes tsn script schedule_appointmetn.ctp
The view is set on views _shared catalog schedule_appointment.ctp

> tire.has_mailin_rebate


## PROMOTIONS HOW THEY ARE DISPLAYED ON THE SITE FOR TSN
>
app/views/brain/themes/tsn/css/catalog.less
app/views/brain/themes/tsn/scripts/catalog/tires.ctp 

## CATALOG RESULTS FOR TSN 
>
app/views/_brain/themes/tsn/goodyear_landing/index.ctp
app/views/_brain/themes/tsn/scripts/catalog/tires.ctp

### GET FRENCH VERSION OF MYCATEGORY 

> I needed to get the French version of the Category string, I couldn't do this with JS, so I needed to foreach it in PHP inside of catalog controller.
```
if($GLOBALS['_Settings']['locale'] != 'en-us') {
    foreach($products as $key => $product) {
        foreach ($product['MyCategory'] as $k => $category) {
            $products[$key]['MyCategory'][$k]['name'] = __($category['name'], true);
        }
    }
}
```

# HOW TO RUN A SHELL SCRIPT
>
Path wherer Kenny and I ran it. 
> /var/www/rpmsolutions
>
 This comand is saying link a symlink from
 > /opt/phpfarm/inst/php-5.3.29/bin/php 
>
to 
 > /usr/bin/php 
>
this will allow me to run php 5.3 from anywhere I want.

####Symlink Command:
>
```bash
sudo ln -s /opt/phpfarm/inst/php-5.3.29/bin/php /usr/bin/php 
```
> Unlink 
````bash
sudo unlink link/path
````
####Available Shells:
>
./cake_run_dev will show you all of the bash scripts that have been created. 
cake_run_dev will run shell scripts that have been created.

Paths to Shells

admin/vendors/shells/:
  >>test 
  locale
  many more...

vendors/shells/:
  >>none

cake/console/libs/:
  >>console
  i18n
  bake
  many more...

### UPDATE STRINGS FOR FRENCH SHELL SCRIPT 
>To generate the -tsn-only for French
```            admin vendors/shells/tsn_summary.php
./cake_run_dev locale generate -tsn-only
```
>>Run i18n method when you wrap new strings.
```
./cake_run_dev locale i18n
```
>To update when you add a new translations then you will see it reflected in the default.po file.
```
./cake_run_dev locale update_locales
```
##__() function
>
When you want the string to be translated and echoed into french wrap the string with Cake's __() function

```bash
__('Some String Here');
```
>
If you want the string return instead of echoed then add a second paramenter of true to the function.

```bash
__('Some String Here', true);
`
`>`
In order for French characters to be converted in the browers the string needs to be injected into the htmlentities function.

```bash
htmlentities('THE STRING', ENT_COMPAT, 'UTF-8');
htmlentities(__('Some String Here', true), ENT_COMPAT, 'UTF-8');
```


##Dyanmic Contact 
$template_email is the template to use inside of the functions. 
Setting debug to true will make it so that you see the email after submitting
``` 
var $debug = true;
```


# Contact US

```
$http.post('/catalog/set_vehicle', {TireGuideRecord: tireGuideRecord})
.success(function(data, status, headers, config) {
    if (data.TG) {                           
        $scope.summary.vehicle.selected_size = data['TG'][0]['TireGuideRecord']['FTC_TireSize'];
        $scope.summary.vehicle.CYEAR = tireGuideRecord.CYEAR;
     $scope.summary.vehicle.MAKE = tireGuideRecord.MAKE;
     $scope.summary.vehicle.MODEL = tireGuideRecord.MODEL;
     $scope.summary.vehicle.OPTION1 = tireGuideRecord.OPTION1;
     $scope.summary.vehicle.year = tireGuideRecord.year_value;
     $scope.summary.vehicle.make = tireGuideRecord.make_value;
     $scope.summary.vehicle.model = tireGuideRecord.model_value;
     $scope.summary.vehicle.option = tireGuideRecord.option_value;

     $scope.data.change_vehicle = false;
     $scope.ajaxSummaryPiece('vehicle', $scope.summary.vehicle);
    }
});
```

##PHP Import method from different classes
>
```
App::import('Helper', 'LocaleDate');
$this->LocaleDateHelper = new LocaleDateHelper;
```
In the locale helper I needed to add an argument for milliseconds which I did by transforming the date to millis with

```
echo 'This is the date '. $this->LocaleDateHelper->getDate(date("n/j/y")) . ' @ ' . $this->LocaleDateHelper->getFormattedTime(strtotime(date('g:i a'))) . '<br>';
```

##ELEMENT RENDERING WITH CAKEPHP
>
You can render an element with Cake and add conditions to the query for the data that will be placed in the element.
```bash
<?=$this->element('promo-box-promotions', array(
    'conditions' => array(
        'Promotion.featured' => true,
        'Promotion.link_to' => 'Rebate',
        'Promotion.show_on_home_page' => true
    ),
    'limit' => (isset($limit)
        ? $limit
        : 2
    ),
    'list' => true,
))?>
```
##CakePHP ELEMENTS
> CakePHP elements - An element is a mini-view that can be included in other views and even elements.  They are outputed using the element method of the view.

```bash
$this->element('elementName', arrayOfConditions);
```

> ```arrayOfConditions``` - Pass data to an element through the element’s second argument.  The second argument combines options for the element with the data for the element to pass.


####Querying Fields 
Querying fields are Conditional you don't have to pull in the entire table. 

```bash

$query['fields'] = array(
    'Promotion.link_to',
    'Promotion.image_home_page_id',
    'Promotion.id',
    'Promotion.slug',
    'Promotion.title',
    'Promotion.url',
    'Promotion.order',
    'Promotion.ends',
    'Promotion.show_on_home_page',
    'Promotion.show_on_specials_page',
    'Promotion.teaser',
    'Promotion.button_email',
    'Promotion.button_print',
    'Promotion.featured',
    'ImageHomePage.name',
    'ImageHomePage.type',
);

$promotions = $this->Promotion->find('all', $query);
```

#What is the _shared folder
>
The shared folder has a symlink to the webroot of the website. 
> You can see where the /shared/*.* links to inside of ```/app/config/routes.php```
```
//shared file routing
Router::connect('/shared_js/*', array('controller' => 'shared_files'));
```
>
Look at the queries on myInstallers pages and we figured out that the only thing needed was name and url.

INstaller controller 
and in the view you will find the fucntion FIND to query contentsSubcatergory
The class in app controller for promotion calls a qurry then sets 'promotions' and it can be acessed in the controller as $this->$promotion in the view it is $promotion
this->$var add to viewVars
MyInstaller Controlle -> app_controller setAssociatedPromotions() is called in appController.
On the contents_controller inside of the view removed the same setAssociatedPromotions();

What is in this->viewVars array inside of function view()

##Services

> this is the MainService page.
```
/app/views/_brain/themes/tsn/pages/services.ctp
```

> this is the IndividualService page
```
/app/views/_brain/themes/tsn/contents/category.ctp
```

## VAGRANTFILE 

```
config.vm.synced_folder "../", "/var/www/rpmsolutions", id: "rpmsolutions", :sync_type => "default", :owner => "vagrant", :group => "www-data", :mount_options => ["dmode=775,fmode=664"]

```

# Kenny Presentation
>
NGINX - pageSpeed is compiled into NGINX
Apche + ModPHP
PHP + Hacked up CakePHP
Percona MySQL
Multi-Tendant App 1.1.2 beta Cake is at version 3.

### Concepts
>
5 Request at a time.
Bundle JS - Webpack.
Horizontal DB servers

### HTTP Request
>
NGINX recieves traffic
  Do I have a static file
    Should I cache it in ram with pageSpeed
    So pageSpeed gets added in the URL
  Dynamic
    Proxy it to a dynamic server
  NGINX with Damian FPM
    This will make it faster?

/promotions
/promotions cached
/prmotions Apache

####APACHE 
  >
  Routes through CakePHP
  cake runs the controller
  DB

####REDIS/MEMCACHE
  >
  Third precence that will be introduced

opt/www is a file server which only provides a file system.

###3 steps to beef up
>
1. Beef Up NGINX
2. Add an NGINX FPM server instead of Apache
3. Add a Payload.

Cached === 'To the file system' (This is true).


`

## JETBEANS Config
>
Multiple Select use to be ```alt+j``` now it is ```ctrl+d``` select all ```ctrl+shift+alt+d```
show console ```alt+f12```



# Adding shell scirpts and cron jobs
> 
* Cron jobs are added to the Sched Task server as root.
* add a shell script inside of admin -  https://github.com/TCSTech/rpmsolutions/compare/rpmsol-963#diff-e6da02dfc0489e98b5d5753c86324961
* Adding Shells to client reports
* sql -> query (SELECT 'columns'  FROM 'table', false='do not cache the query')
* We have all of the functionality to query the database in each_client.php class inside of admin

# ViewVars 
> You can view the viewVars that are avaliable on the page.
```d($this->viewVars['myInstallers']); 
```
> can be used in the page as 
```$myInstallers```

#Promotions
>Broken
```https://github.com/TCSTech/rpmsolutions/commit/5965222986b2323d96d6c967111b834100264c16```
>Fixed
```https://github.com/TCSTech/rpmsolutions/pull/6221/files```

##Programing
> If we understand that programming languages are tools to teach programming concepts, we have conflicst between concepts and syntax.
-Ashley Williams

>Writing is nature's way of letting you know how sloppy your thinking is
-Ashley Williams

>Teaching is nature’s way of letting you know how sloppy your understanding is.
-Ashley Williams

#Beast View Online
On Catalog Controller the ```function schedule_appointment``` is what is called when you request a quote on the beast form.  That function get the promotion IDs and the Product Id and disableAppScripts
>You then create a new view for the email ```$View = new View($this);```
>Uses a view as its email template, passed into ['DynamicValidates']['schedule_appointment_email_html']
```$this->data['DynamicValidates']['schedule_appointment_email_html'] = $html;```
>['DynamicValidates'] object gets subject lines added to it.
Then it send you to the confirmation page ```function confirm_schedule_appointment```
Confirmation codes reads summary, checks for appointments, pulls promotion data. AND
```if ($this->Session->check('email_schedule_appointment_html')) {
            $html = $this->Session->read('email_schedule_appointment_html');
        }```

>  I need to know if I have access to the value of ```$foriegn_hash_id_link;``` in the confirmation page. 

#Tire Size
```MyProduct.quick_search``` in the $summary has the tire size without the lettters.
>Code changed https://github.com/TCSTech/rpmsolutions/pull/6313


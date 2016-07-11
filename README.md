# NetSuiteBundle for Symfony 3 

All the hard work has been done, full credit to @ryanwinchester

## Installation 
```composer
composer require ryanwinchester/netsuite-php
```
## Configuration 
```yaml
  netsuite:
        endpoint: 2016_1
        host: https://webservices.netsuite.com
        email: you@youremail.com
        password: yourpassword
        role: 3
        account: accountId
        app_id: appId
        logging: false
 ```
 Refer [ryanwinchester/netsuite-php](https://github.com/ryanwinchester/netsuite-php) for examples and logging configuration
 
## Example Usage
 
 ```php
    /**
     * @Route("/customer/{id}", name="customer")
     */
    public function customerAction(Request $request)
    {
        $customerId = $request->attributes->get('id');

        $config = $this->container->getParameter('netsuite');
        $service = new NetSuiteService($config);

        $NSrequest = new GetRequest();
        $NSrequest->baseRef = new RecordRef();
        $NSrequest->baseRef->internalId = $customerId;
        $NSrequest->baseRef->type = "customer";
        $getResponse = $service->get($NSrequest);

        if ( ! $getResponse->readResponse->status->isSuccess) {
            $customer = "GET ERROR";
        } else {
            $customer = $getResponse->readResponse->record;
        }

        return $this->render('default/customer.html.twig', [
            'customer'=> $customer,
        ]);
    }
  ```  
    
 And that is all there is to it 
  

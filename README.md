# lightning-component
#Test  Build a basic lightning component that can query a list of 10 most recently created accounts and display it using a lightning app. 

public with sharing class AccountController {
    @AuraEnabled(cacheable=true)
    public static List<Account> getRecentAccounts() {
        return [SELECT Id, Name, CreatedDate FROM Account ORDER BY CreatedDate DESC LIMIT 10];
    }
}


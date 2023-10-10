recentAccountsList.html
----------------------------------------------------------------------------
<template>
    <table class="slds-table slds-table_bordered slds-table_cell-buffer">
        <thead>
            <tr>
                <th>Name</th>
                <th>Industry</th>
                <th>Phone</th>
                <th>Website</th>
            </tr>
        </thead>
        <tbody>
            <template for:each={accounts} for:item="account">
                <tr key={account.Id}>
                    <td>{account.Name}</td>
                    <td>{account.Industry}</td>
                    <td>{account.Phone}</td>
                    <td>
                        <a href={account.Website} target="_blank" rel="noopener noreferrer">
                            {account.Website}
                        </a>
                    </td>
                </tr>
            </template>
        </tbody>
    </table>
</template>


recentAccountsList.js
------------------------------------------------------
import { LightningElement, wire } from 'lwc';
import getRecentAccounts from '@salesforce/apex/RecentAccountsController.getRecentAccounts';

export default class RecentAccountsList extends LightningElement {
    accounts = [];

    @wire(getRecentAccounts)
    wiredAccounts({ error, data }) {
        if (data) {
            this.accounts = data;
        } else if (error) {
            console.error('Error loading recent accounts:', error);
        }
    }
}

recentAccountsList.css
/* Add custom styles for the table */
.slds-table {
    width: 100%;
    border-collapse: collapse;
    margin: 20px 0;
    font-size: 16px;
}

.slds-table th,
.slds-table td {
    padding: 10px;
    text-align: left;
    border: 1px solid #dcdcdc;
}

.slds-table th {
    background-color: #f2f2f2;
}

.slds-table td a {
    color: #0070d2;
    text-decoration: none;
}

.slds-table td a:hover {
    text-decoration: underline;
}

RecentAccountsController
public with sharing class RecentAccountsController {
    @AuraEnabled(cacheable=true)
    public static List<Account> getRecentAccounts() {
        return [SELECT Id, Name FROM Account ORDER BY CreatedDate DESC LIMIT 10];
    }
}

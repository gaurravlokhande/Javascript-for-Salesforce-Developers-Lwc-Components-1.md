 ## Delete any Record And Refresh Data: means all data get updated with existing records without refresh page: through import refresh and deleterecord
HTML:

```
<template>
    <lightning-card title="Contact List">
        <template if:true={wiredData.data}>
            <template for:each={wiredData.data} for:item="contact">
                <div key={contact.Id}>
                    <p>{contact.Name}</p>
                    <lightning-button
                        label="Delete"
                        variant="destructive"
                        onclick={handleDeleteContact}
                        data-contact-id={contact.Id}
                    ></lightning-button>
                </div>
            </template>
        </template>
    </lightning-card>
</template>
```
JS:

```
import { LightningElement, wire, track } from 'lwc';
import { refreshApex } from '@salesforce/apex';
import getContacts from '@salesforce/apex/ContactController.getContacts';
import { deleteRecord } from 'lightning/uiRecordApi';

export default class ContactList extends LightningElement {
    // Declare a property to hold the wired data
    @track wiredData;

    @wire(getContacts)
    wiredContacts(result) {
        this.wiredData = result;
    }

    // Function to delete a contact and then refresh the wired data
    async handleDeleteContact(event) {
        const contactId = event.currentTarget.dataset.contactId;
        await deleteRecord({ this.contactId });

        // Refresh the wired data to reflect the deletion
        return refreshApex(this.wiredData);
    }
}
````

JS:
```

// Automatically refresh data when the component is initialized
connectedCallback() {
    this.refreshData();


@track wiredData;

@wire(getContacts)
wiredContacts(result) {
    this.wiredData = result;
}

// Function to refresh the wired data
refreshData() {
    return refreshApex(this.wiredData);
}
}
```
## used in a project

```
async onDeleteContact(event) {
    const contactId = event.currentTarget.dataset.id;

    await deleteRecord(contactId)
        .then((result) => {
            this.dispatchEvent(new ShowToastEvent({
                title: "Contact Deleted Successfully",
                variant: "success"
            }));
             this.refreshdata();
        })
        .catch((error) => {
            this.dispatchEvent(new ShowToastEvent({
                title: "Error While Deleting Contact",
                variant: "error"
            }));
        });
}


    refreshdata() {
    return refreshApex(this.StoreContacts);
}

````
   
       
    


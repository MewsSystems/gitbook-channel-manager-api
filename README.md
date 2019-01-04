# Introduction

The connection between Mews \(as a PMS\) and a channel manager is a 2-way connection.   
The following document defines both the Mews \(as PMS\) side, and also defines the API on the channel manager side.   
The Mews side mainly accepts bookings from the channel manager, while the channel manager side accepts inventory updates from the PMS and then distributes them to connected OTAs.

2-way connection means that both sides of the connection - channel manager and PMS - are required to accept data from the other side in order to correctly connect a PMS through channel manager to all the OTAs.  
So both sides need to have an open API which the other side has to connect to.  
Channel manager side accepts Inventory updates \(prices, availability, restrictions\) and booking confirmations.  
PMS side accepts bookings.

If you encounter any issue with the API, have a question or any other other request, please contact [integrations@mewssystems.com](mailto://integrations@mewssystems.com).

## Contents

* [General remarks](general-remarks.md)
  * [Requests](general-remarks.md#requests)
  * [Responses](general-remarks.md#responses)   
  * [Setup process](general-remarks.md#setup-process)
    * [Integration setup](general-remarks.md#integration-setup)
    * [Connect new property](general-remarks.md#connect-new-property)
    * [Certification](general-remarks.md#certification)
* [Mews API](mews-api.md#mews-api)
  * [Environments](mews-api.md#environments)
    * [Test Environment](mews-api.md#test-environment)
      * [Test Credit Cards](mews-api.md#test-credit-cards)
    * [Production environment](mews-api.md#production-environment)
  * [Operations](mews-api.md#operations)
    * [Get Properties](mews-api.md#get-properties)
    * [Get Configuration](mews-api.md#get-configuration)
    * [Set Inventory](mews-api.md#set-inventory)
    * [Request ARI Update](mews-api.md#request-ari-update)
    * [Process Group](mews-api.md#process-group)
* [Channel Manager API](channel-manager-api.md)
  * [Mews Inventory Update Modes](channel-manager-api.md#mews-inventory-update-modes)
    * [Full Inventory Update Mode](channel-manager-api.md#full-inventory-update-mode)
    * [Delta Inventory Update Mode](channel-manager-api.md#delta-inventory-update-mode)
  * [Expected Operations](channel-manager-api.md#expected-operations)
    * [Update Prices](channel-manager-api.md#update-prices)
    * [Update Availability](channel-manager-api.md#update-availability)
    * [Update Restrictions](channel-manager-api.md#update-restrictions)
    * [Confirm Booking](channel-manager-api.md#confirm-booking)
    * [Change Notification](channel-manager-api.md#change-notification)
* [Certification](certification.md)
  * [Prerequisites](certification.md#prerequisites)
  * [Testing scenario](certification.md#testing-scenario)
  * [Evaluation](certification.md#evaluation)


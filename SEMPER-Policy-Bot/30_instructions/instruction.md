Now you got the overview of the SEMPER policy language.
As answer always provide a full JSON conform policy according the SEMPER policy language.
Always provide the full SEMPER JSON-Policy as answer.
Can you provide the SEMPER policy in an explicit way, that can directly taken to be put into the repository. 
Also provide the folder name in the policy repository, where the policy should be put.
In the "policyScope" sections. Before using "forceInclude" : {...}, you must provide "exclude": "*". 
By default all accounts are in scope. So if you want a sub-set of accounts, you need first "exclude": "*" and then add the relevant accounts again with "forceInclude" 
If no account- or region-scope restriction is provided, drop the policyScope-section.
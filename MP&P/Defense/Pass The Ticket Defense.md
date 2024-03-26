- #important  #detection Event ID 4769 is generated on the Domain Controller when using a golden ticket after the KRBTGT password has been reset twice, as mentioned in the mitigation section. The status code 0x1F indicates the action has failed due to “Integrity check on decrypted field failed” and indicates misuse by a previously invalidated golden ticket. 
- #detection Audit all Kerberos authentication and credential use events and review for discrepancies. Unusual remote authentication events that correlate with other suspicious activity (such as writing and executing binaries) may indicate malicious activity.

- #mitigation  For containing the impact of a previously generated golden ticket, reset the built-in KRBTGT account password twice, which will invalidate any existing golden tickets that have been created with the KRBTGT hash and other Kerberos tickets derived from it.
- #mitigation  Ensure that local administrator accounts have complex, unique passwords.
- #mitigation  Limit domain admin account permissions to domain controllers and limited servers. Delegate other admin functions to separate accounts.
- #mitigation  Do not allow a user to be a local administrator for multiple systems.

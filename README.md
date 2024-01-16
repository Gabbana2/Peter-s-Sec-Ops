# Peter-s-Sec-Ops


//This function lists all Known Exploited Vulnerabilities as classified by CISA. The parameter StartYear determines from which year on you want to list the vulnerabilities
let ListCISAExploitedVulnerabilites = (StartYear:long) { 
    let KnowExploitesVulnsCISA = externaldata(cveID: string, vendorProject: string, product: string, vulnerabilityName: string, dateAdded: datetime, shortDescription: string, requiredAction: string, dueDate: datetime, notes: string)[@"https://www.cisa.gov/sites/default/files/csv/known_exploited_vulnerabilities.csv"] with (format="csv", ignoreFirstRecord=True);
    KnowExploitesVulnsCISA
    | extend DueDateExceededByDays = datetime_diff('day', now(), dueDate) 
    | extend ReleaseYear = tolong(extract(@'CVE-(.*?)-', 1, cveID))
    | where ReleaseYear >= StartYear
};
// Example only list from 2023 or newer
ListCISAExploitedVulnerabilites(2023);



# README.md

IMPORTANT: Once you've cloned this to your forked repository, ensure that you continuously update this document as you complete each task to demonstrate your ongoing progress.

Please include your shared repository link here:

Example:
Choiru's shared repository: https://github.com/choiruzain-latrobe/Assignment2.git


Make sure for **your case it is in Private**
## Access Database
1 **Plsql Cheat Sheet:**
You can refer to the PostgreSQL cheat sheet [here](https://www.postgresqltutorial.com/postgresql-cheat-sheet/).

2 **Know the Container ID:**
To find out the container ID, execute the following command:
   ```bash
   docker ps
    9958a3a534c9   testsystem-nginx           "/docker-entrypoint.…"   6 minutes ago   Up 6 minutes   0.0.0.0:80->80/tcp   testsystem-nginx-1
    53121618baa4   testsystem-frontend        "docker-entrypoint.s…"   6 minutes ago   Up 6 minutes   3000/tcp             testsystem-frontend-1
    c89e46ac94b0   testsystem-api             "docker-entrypoint.s…"   6 minutes ago   Up 6 minutes   5000/tcp             testsystem-api-1
    9f4aea7cf538   postgres:15.3-alpine3.18   "docker-entrypoint.s…"   6 minutes ago   Up 6 minutes   5432/tcp             testsystem-db-1
   ```
3. Running the application

**docker compose command:**
   ```bash
   docker compose up --build
   ```

4 **Access postgreSQL in the container:**
Once you have the container ID, you can execute the container using the following command:
You will see the example of running the PostgreSQL inside the container.
   ```bash
   docker exec -it testsystem-db-1 psql -U postgres
   choiruzain@MacMarichoy TestSystem % docker exec -it testsystem-db-1 psql -U postgres                                       
   psql (15.3)
   Type "help" for help.
   
   postgres=# \dt
             List of relations
    Schema |   Name   | Type  |  Owner   
   --------+----------+-------+----------
    public | contacts | table | postgres
    public | phones   | table | postgres
   (2 rows)
  
    postgres=# select * from contacts;
    id |  name  |         createdAt         |         updatedAt         
   ----+--------+---------------------------+---------------------------
     1 | Helmut | 2024-08-08 11:57:57.88+00 | 2024-08-08 11:57:57.88+00
    (1 row)
    postgres=# select * from phones;
    id | phone_type |   number    | contactId |         createdAt          |         updatedAt          
   ----+------------+-------------+-----------+----------------------------+----------------------------
     1 | Work       | 081431      |         1 | 2024-08-08 11:59:04.386+00 | 2024-08-08 11:59:04.386+00


postgres=# select * from contacts;
   ```
Replace `container_ID` with the actual ID of the container you want to execute.

## Executing API

### Contact API


1. Add contacts API  (POST)
```bash
http post http://localhost/api/contacts name="Choiru"
        
choiruzain@MacMarichoy-7 TestSystem % http post http://localhost/api/contacts name="Choiru"
HTTP/1.1 200 OK
Access-Control-Allow-Origin: http://localhost:3000
Connection: keep-alive
Content-Length: 102
Content-Type: application/json; charset=utf-8
Date: Thu, 08 Aug 2024 21:01:53 GMT
ETag: W/"66-FmPYAaIkyQoroDwP2JsAZjWTAxs"
Server: nginx/1.25.1
Vary: Origin
X-Powered-By: Express

{
"createdAt": "2024-08-08T21:01:53.017Z",
"id": 1,
"name": "Choiru",
"updatedAt": "2024-08-08T21:01:53.017Z"
}

```
2 Get contacts API  (GET)

```bash
http get http://localhost/api/contacts


choiruzain@MacMarichoy-7 TestSystem % http get http://localhost/api/contacts
HTTP/1.1 200 OK
Access-Control-Allow-Origin: http://localhost:3000
Connection: keep-alive
Content-Length: 104
Content-Type: application/json; charset=utf-8
Date: Thu, 08 Aug 2024 21:04:58 GMT
ETag: W/"68-V+4KuL2xahYt8YAkKG6rKdR7wHg"
Server: nginx/1.25.1
Vary: Origin
X-Powered-By: Express

[
{
"createdAt": "2024-08-08T21:01:53.017Z",
"id": 1,
"name": "Choiru",
"updatedAt": "2024-08-08T21:01:53.017Z"
}
]


```
3. Show/create the API commmand to delete the contacts (DELETE)

```bash





```

4. Show/create the API command to edit the contacts (PUT)
```
http get http://localhost/api/contacts/1/phones

```

### Phone API


##Task 1
1.1 Change code in line 40 in contact.js
1.2 Change line 36 in newphone.js
1.3 change line 34 in newphone.js
1.4 Line 19 in phone.js

##Task 2
2.1.Show the API command for “Show Contact” and provide a screenshot of the output 

2.2. Show the API command for “Add Contact” and provide a screenshot of the output 
2.3. Show the API command for “Delete Contact” and provide a screenshot of the output 
2.4. Show the API command for “Update Contact” and provide a screenshot of the output 
2.5. Show the API command for “Show Phone” and provide a screenshot of the output 
2.6. Show the API command for “Add Phone” and provide a screenshot of the output 
2.7. Show the API command for “Delete Phone” and provide a screenshot of the output 
2.8. Show the API command for “Update Phone” and provide a screenshot of the output 


import React, { useState } from 'react';

const NewCompany = ({ setCompanies }) => {
    const [name, setName] = useState('');
    const [address, setAddress] = useState('');
    const [contactId, setContactId] = useState('');

    const createCompany = async (e) => {
        e.preventDefault();
        const response = await fetch('http://localhost/api/companies', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({ company_name: name, company_address: address, contact_id: contactId })
        });

        const newCompany = await response.json();
        setCompanies(prevCompanies => [...prevCompanies, newCompany]);

        setName('');
        setAddress('');
        setContactId('');
    };

    return (
        <form onSubmit={createCompany}>
            <h3>Create New Company</h3>
            <input
                type="text"
                placeholder="Company Name"
                value={name}
                onChange={(e) => setName(e.target.value)}
                required
            />
            <input
                type="text"
                placeholder="Company Address"
                value={address}
                onChange={(e) => setAddress(e.target.value)}
                required
            />
            <input
                type="number"
                placeholder="Contact ID"
                value={contactId}
                onChange={(e) => setContactId(e.target.value)}
                required
            />
            <button type="submit">Add Company</button>
        </form>
    );
};

export default NewCompany;

## EDit Comapny
import React, { useState } from 'react';

const EditCompany = ({ company, setIsEditing, setCompanies }) => {
    const [name, setName] = useState(company.company_name);
    const [address, setAddress] = useState(company.company_address);

    const updateCompany = async (e) => {
        e.preventDefault();
        const response = await fetch(`http://localhost/api/companies/${company.company_id}`, {
            method: 'PUT',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({ company_name: name, company_address: address, contact_id: company.contact_id })
        });

        const updatedCompany = await response.json();
        setCompanies(prevCompanies => prevCompanies.map(c => c.company_id === updatedCompany.company_id ? updatedCompany : c));
        setIsEditing(false);
    };

    return (
        <form onSubmit={updateCompany}>
            <input
                type="text"
                placeholder="Company Name"
                value={name}
                onChange={(e) => setName(e.target.value)}
                required
            />
            <input
                type="text"
                placeholder="Company Address"
                value={address}
                onChange={(e) => setAddress(e.target.value)}
                required
            />
            <button type="submit">Save</button>
            <button onClick={() => setIsEditing(false)}>Cancel</button>
        </form>
    );
};

export default EditCompany;
<div className="App">
      <h1>Company Management</h1>
      <CompanyList />
    </div>


<div style={expandStyle}>
            <hr />
            <PhoneList phones={phones} setPhones={setPhones} contact={contact} />
            <hr />
            <CompanyList companies={companies} setCompanies={setCompanies} contact={contact} />
        </div>

<input
        type="text"
        placeholder="Company Name"
        value={companyName}
        onChange={(e) => setCompanyName(e.target.value)}
      />
      <input
        type="text"
        placeholder="Company Address"
        value={companyAddress}
        onChange={(e) => setCompanyAddress(e.target.value)}
      />

function Company(props) {
    const {contact, company, companies, setCompanies} = props;

    async function deleteCompany() {
        const response = await fetch('http://localhost/api/contacts/' + contact.id + '/companies/' + company.company_id, {
            method: 'DELETE',
        });

        let newCompanies = companies.filter((p) => {
            return p.company_id !== company.company_id;
        });

        setCompanies(newCompanies);
    }

	return ( 
		<tr>
            <td>{ company.company_name }</td>
            <td>{ company.company_address }</td>
            <td style={
                {
                    width: '14px',
                }
            }><button className="button red" onClick={deleteCompany}>Delete Company</button></td>
        </tr>
	);
}

export default Company;

# update

import React, { useState } from 'react';

function Company(props) {
  const { contact, company, companies, setCompanies } = props;
  const [isEditing, setIsEditing] = useState(false);
  const [companyName, setCompanyName] = useState(company.company_name);
  const [companyAddress, setCompanyAddress] = useState(company.company_address);

  async function deleteCompany() {
    const response = await fetch('http://localhost:5000/api/companies/' + company.company_id, {
      method: 'DELETE',
    });

    if (response.ok) {
      let newCompanies = companies.filter((p) => {
        return p.company_id !== company.company_id;
      });
      setCompanies(newCompanies);
    } else {
      console.error('Failed to delete the company');
    }
  }

  async function updateCompany() {
    const response = await fetch('http://localhost:5000/api/companies/' + company.company_id, {
      method: 'PUT',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        company_name: companyName,
        company_address: companyAddress,
        contact_id: contact.id
      })
    });

    if (response.ok) {
      const updatedCompany = await response.json();
      let newCompanies = companies.map((c) => (c.company_id === company.company_id ? updatedCompany : c));
      setCompanies(newCompanies);
      setIsEditing(false);
    } else {
      console.error('Failed to update the company');
    }
  }

  return (
    <tr>
      {isEditing ? (
        <>
          <td>
            <input
              type="text"
              value={companyName}
              onChange={(e) => setCompanyName(e.target.value)}
            />
          </td>
          <td>
            <input
              type="text"
              value={companyAddress}
              onChange={(e) => setCompanyAddress(e.target.value)}
            />
          </td>
          <td style={{ width: '14px' }}>
            <button className="button green" onClick={updateCompany}>Save</button>
            <button className="button red" onClick={() => setIsEditing(false)}>Cancel</button>
          </td>
        </>
      ) : (
        <>
          <td>{company.company_name}</td>
          <td>{company.company_address}</td>
          <td style={{ width: '14px' }}>
            <button className="button blue" onClick={() => setIsEditing(true)}>Edit</button>
            <button className="button red" onClick={deleteCompany}>Delete</button>
          </td>
        </>
      )}
    </tr>
  );
}

export default Company;

# New
import React, { useState } from 'react';

function Company(props) {
    const { contact, company, companies, setCompanies } = props;

    // State to handle editing
    const [isEditing, setIsEditing] = useState(false);
    const [companyName, setCompanyName] = useState(company.company_name);
    const [companyAddress, setCompanyAddress] = useState(company.company_address);

    // Delete Company Function
    async function deleteCompany() {
        const response = await fetch('http://localhost/api/contacts/' + contact.id + '/companies/' + company.company_id, {
            method: 'DELETE',
        });

        let newCompanies = companies.filter((p) => {
            return p.company_id !== company.company_id;
        });

        setCompanies(newCompanies);
    }

    // Update Company Function
    async function updateCompany(e) {
        e.preventDefault(); // Prevent form submission

        const response = await fetch('http://localhost/api/contacts/' + contact.id + '/companies/' + company.company_id, {
            method: 'PUT',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify({
                company_name: companyName,
                company_address: companyAddress,
            }),
        });

        const updatedCompany = await response.json();

        // Update the state with the new company data
        let updatedCompanies = companies.map((c) =>
            c.company_id === company.company_id ? updatedCompany : c
        );

        setCompanies(updatedCompanies);
        setIsEditing(false); // Exit edit mode
    }

    return (
        <tr>
            {isEditing ? (
                // Edit mode: render a form for updating company details
                <>
                    <td>
                        <input
                            type="text"
                            value={companyName}
                            onChange={(e) => setCompanyName(e.target.value)}
                        />
                    </td>
                    <td>
                        <input
                            type="text"
                            value={companyAddress}
                            onChange={(e) => setCompanyAddress(e.target.value)}
                        />
                    </td>
                    <td style={{ width: '14px' }}>
                        <button className="button green" onClick={updateCompany}>
                            Save
                        </button>
                        <button
                            className="button yellow"
                            onClick={() => setIsEditing(false)}
                        >
                            Cancel
                        </button>
                    </td>
                </>
            ) : (
                // View mode: render the company details
                <>
                    <td>{company.company_name}</td>
                    <td>{company.company_address}</td>
                    <td style={{ width: '14px' }}>
                        <button className="button blue" onClick={() => setIsEditing(true)}>
                            Edit
                        </button>
                        <button className="button red" onClick={deleteCompany}>
                            Delete Company
                        </button>
                    </td>
                </>
            )}
        </tr>
    );
}

export default Company;






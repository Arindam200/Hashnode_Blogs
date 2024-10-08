---
title: "How to Add RBAC Authorization in Next.js"
seoDescription: "Learn to add secure and scalable RBAC authorization to your Next.js app using Permit.io. Step-by-step guide and tutorial included"
datePublished: Wed Sep 18 2024 06:36:39 GMT+0000 (Coordinated Universal Time)
cuid: cm17hpi4f000809jub8vp2ncj
slug: how-to-add-rbac-authorization-in-nextjs
canonical: https://javascript.plainenglish.io/how-to-add-rbac-authorization-in-next-js-1d5e50cbad29
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1726641249937/53534296-06c4-4cf8-b9c5-9f0d529b662d.png
tags: web-development, authorization, nextjs, permit

---

Authorization, the process in our applications that determines what users can perform on which resources, is a crucial requirement for every application. Implementing [Role-Based Access Control (RBAC)](https://www.permit.io/blog/an-introduction-to-role-based-access-control) is a simple way to manage and control authorization in our applications.

In this article, we’ll discuss the process of adding RBAC Authorization to a Next.js application in a way that’s both secure and scalable.

We’ll start by setting up a basic Todo application in Next.js and implementing JWTs for user authentication. Next, we’ll configure an RBAC policy using [Permit.io](http://Permit.io), sync our users, and upgrade a user’s role to admin with full permissions.

By the end of this tutorial, you'll have a clear understanding of how to secure your Next.js application with RBAC, giving you complete control over what each user can access based on their role.

Let’s Begin!

## Setting up a Basic Next.js Project

To get started, let's create a new Next.js project. To save time (and assuming you understand the basics of Next.js), we already have a starter project set up where you'll find the simple to-do app we’ll be using in this tutorial.

1. Make sure you have Node.js and npm installed on your machine. You can download them from the official [Node.js website](https://nodejs.org/en/)
    
2. Open a terminal window and create a new Next.js project using the following command:
    

```jsx
git clone https://github.com/Arindam200/Permit-RBAC
```

This will clone a Next.js project with the default settings containing the starter code.

3. After creating the project, navigate to the project directory by running:
    

```jsx
cd rbac-permit-starter-next.js
```

4. Next, we'll install the required dependencies by running:
    

```jsx
npm install
```

Now that we’ve successfully set up our Next.js project locally, we can go ahead and implement a basic RBAC model.

## Authenticating Users with JWTs in Next.js

Before starting with [authorization,](https://www.permit.io/blog/authentication-vs-authorization) we need to authenticate our users.

The authentication phase verifies and provides each user with a unique identity, which helps differentiate one user from the other. Authorization restricts users to only performing actions they are authorized to perform within the application.

To keep our application simple, we’re using dummy JWTs of two users that demonstrate admin and user permissions. The same method will work for implementation with any authentication provider that provides JWTs, such as Clerk, Auth0, or Kinde.

The dummy user details are as follows :

1. Create two users with the following credentials:
    
    * **User 1** (Admin):
        
        * **Key**: 1
            
        * **Email**: [admin@gmail.com](mailto:admin@gmail.com)
            
        * **First Name**: Project
            
        * **Last Name**: Admin
            
        * **Tenant**: Todo-tenant (or your created tenant)
            
        * **Role**: Admin
            
    * **User 2** (Regular User):
        
        * **Key**: 2
            
        * **Email**: [user@gmail.com](mailto:user@gmail.com)
            
        * **First Name**: Project
            
        * **Last Name**: User
            
        * **Tenant**: Todo-tenant (or your created tenant)
            
        * **Role**: User
            

In your code editor, navigate to code inside the file src &gt; data &gt; sampleData.js

```bash
 const sampleData = [
	 "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MSwibmFtZSI6IlByb2plY3QgQWRtaW4iLCJlbWFpbCI6ImFkbWluQGdtYWlsLmNvbSJ9.KtQpee_bZF_Sx0t87trx8-ljuE3SwJ7SZYeYzZO-694", 
  "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MiwibmFtZSI6IlByb2plY3QgVXNlciIsImVtYWlsIjoidXNlckBnbWFpbC5jb20ifQ.FVG2dcFVsOy1cmzImHqtbeJ0mnT1h4aCSN7aSPq9Xew",
                                                                       
	];
export default sampleData;
```

This contains JWTs for both users with the above-written credentials. We will decode these JWTs to obtain user information.

### **Authorization Anti Pattern**

Developers often use the following imperative, conditional statements to enforce RBAC rules.

![Code Snippets](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/y4gb933pq9tw23f4t7yu.png align="left")

In this example, we have a user object with an ID and a role property.

The `deleteUser` function checks if the current user's role is 'admin' before allowing him to delete the user.

The updatePost function checks if the user role has been updated.

**Hard-coding authorization rules using imperative** `if` statements creates several issues:

* **Duplication**: Adding authorization in function calls often leads to repeated checks across multiple functions, cluttering the codebase and increasing the risk of errors during updates.
    
* **Complexity**: With more roles and permissions, nested `if` statements become complex and harder to maintain, making the code prone to bugs and difficult to read.
    
* **Inflexibility**: Hard-coded authorization logic requires manual updates for any changes in roles or permissions, which is time-consuming and error-prone.
    

For this tutorial, we'll implement RBAC using [Permit.io](http://Permit.io) - an authorization as a service provider. [Permit.io](http://Permit.io) addresses the drawbacks of hard-coded authorization by offering a more centralized, flexible, and scalable approach to permission management.

## **Configure Basic RBAC Policy in Permit**

To begin configuring permissions, log in to [app.permit.io](http://app.permit.io). Then, we'll create a new workspace for this project.

Follow these steps:

* Create an account in Permit
    
* Enter a workspace name
    
* Provide a workspace key
    
* Click on "Launch your account"
    

![Permit Workspace](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/dliu2khvmgjasy2mxodw.png align="left")

Next, we need to create a resource. A resource is any element in our application we need to protect or manage access to. For example, in this application, Todo is a resource that represents our tasks that need to be managed and protected.

Let’s create a resource named **Todo**

1. Go to the Policy page and click Create &gt; Resource
    
2. Create the following resource
    
3. Assign five actions (Create, Read, Update, Edit, Delete) related to it:
    

![TODO Resource](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/wq6ha32iw1sn6qa4sgu4.png align="left")

Next, we’ll create a role. Roles are an easy way to group permissions and assign them to users or other entities.

1. Go to the Policy page and click Create &gt; Role
    
2. Add the following roles, with actions as depicted below:
    
    * Admin
        
    * User
        
3. Save these changes.
    

![Roles](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/9q1q79iqnolvktir2bvx.png align="left")

Next, we’ll go to the “Policy Editor” section and set the policy. Before setting the policy, let’s discuss about the actions:

1. **Create**: Add a new task to the To-Do list.
    
2. **Read**: View the details of a task.
    
3. **Update**: Mark a task as done or not done.
    
4. **Edit**: Change the task's name, deadline, or priority.
    
5. **Delete**: Remove a task from the To-Do list.
    

We need to update our policy so that an Admin has the authority to perform all the actions, while a User can only perform the Read and Update actions.

To create this policy, go to the Policy Editor, check all the checkboxes as shown below, and save changes.

![Policy Dashboard](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/d0ilp2ep8jzkp8fkgldk.png align="left")

That’s it. We’ve successfully set up our Role-Based Access Control (RBAC) model, which we can use in our applications.

## Syncing Users to Permit

Now that we have created an RBAC configuration let’s add the dummy users to our [Permit.io](http://Permit.io) directory. Since we're using dummy JWTs, we'll manually add users to the Permit directory. If you have an authentication provider, you can utilize the `syncUser` API to import all your existing users into Permit.

To get started:

1. Go to the permit Directory
    
2. Select the default tenant
    
3. Click on Add User
    

![Add User](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/881ufcakonwtp83m5s0a.png align="left")

1. Fill in the user details as shown below and save them :
    
    * Regular User
        

![Regular User](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/4w8eqearljesueo8mggz.png align="left")

* Admin User
    

![Admin User](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ko0xxcd196xckl3j8omf.png align="left")

Now that we've created some users in Permit, we're ready to start coding.

Let’s start by adding the environment variables:

1. Go to `Settings > API Keys > Developmental secret key`
    
2. Copy the key and paste it in .env as shown below :
    

```jsx
PERMIT_TOKEN=”Copied API KEY”
```

Now let's connect our application with [Permit.io](http://Permit.io) to implement the policy we've set up.

To do this, we'll create a gateway that initializes the [Permit.io](http://Permit.io) client with an API token and a Policy Decision Point (PDP) endpoint. This setup enables the application to perform authorization checks based on predefined access control rules.

1. Navigate to `src > lib > permitProvider.js`
    
2. Initialize the [Permit.io](http://Permit.io) client as follows:
    

```jsx
import { Permit } from 'permitio';
const permit = new Permit({
  // The token used for authenticating with the Permit.io API
  token: process.env.NEXT_PUBLIC_PERMIT_TOKEN,
  // The Policy Decision Point (PDP) URL that Permit.io uses to evaluate policies
  pdp: "https://cloudpdp.api.permit.io",
});
export default permit;
```

Now that we have created two users already, it's time to utilize the existing policy to test how our Role Based Access Control (RBAC) model is working. Before we proceed, let’s discuss how the checks work to grant or restrict access to authorized individuals based on their registered roles.

### Authorization Architecture in Our Application

Let’s understand how we are going to implement authorization in our application:

* Whenever a user wants to perform an operation on the front end, the front end sends the `user_id` and operation name to the backend API.
    
* Our backend then checks if the user has the required permission to perform the operation.
    
* Depending on whether the user is permitted, the backend sends a response to the frontend.
    
* Upon receiving the response, if the user is permitted, the operation is performed; otherwise, it is rejected, with a popup notifying the user that they are not authorized.
    

![Authorization Architecture](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/y88fawwhirsphon8q6jr.png align="left")

### Checking & Validating Permission

Now that we have connected our application and [Permit.io](http://Permit.io), it’s time to see how [Permit.io](http://Permit.io) manages resources.

1. Go to `src > api > check-permission > route.js`
    
2. Add the following code:
    

```bash
import permit from "@/lib/permitProvider";

export async function GET(req) {

  const { searchParams } = new URL(req.url)
  const id = searchParams.get('id');
  const operation = searchParams.get('operation')

  try {    
    const permitted = await permit.check(String(id), String(operation), {
      type: 'TodoTasks',
      tenant: 'todo-tenant',
    });
    
    if (permitted) {
      return Response.json({
        success: true,
        message: "permitted"
      }, {  status: 200 })

    }
      
  } catch (err) {
    console.error("Error checking permissions:", err);
  }
  return Response.json({
            success: false,
            message: "not permitted"
          }, {  status: 403 })
}
```

In the above code, we are extracting query parameters, checking permission with [Permit.io](http://Permit.io) and returning the result.

Here's a step-by-step breakdown:

1. **Extract Query Parameters**:
    
    * The `GET` function is triggered by an HTTP GET request to this API route.
        
    * It retrieves the `id` and `operation` parameters from the request's URL.
        
2. **Check Permission with** [**Permit.io**](http://Permit.io):
    
    * The `permit.check()` method is called to verify if the user identified by `id` is authorized to perform the specified operation (e.g., create, update, delete) on the `TodoTasks` resource within the `todo-tenant`.
        
3. **Return the Result**:
    
    * **Success (200)**: If the user has the necessary permissions, a success response with a status code of 200 is returned.
        
    * **Not Authorized (403)**: If the user does not have the required permissions, a failure response with a status code of 403 is returned.
        

Now take a look at the centralized function making API calls

```jsx
  
const checkPermission = async (operation) => {
    const response = await fetch(`/api/check-permission?id=${identifier}&operation=${operation}`);
    const data = await response.json();
    return data.success;
  };
```

It responds to our designed API route with the required parameters ID & operation.

### Restricting Access to Resources Based on Roles

We have our Todo resource, and as per its policy, individuals with the “Admin” role can perform all five actions: read, create, edit, update, and delete. Those with the “User” role can only perform read and update actions.

**Create:** Function to check if the individual has "edit" permission and updates the specified to-do item with new values.

```jsx
const handleSaveEdit = async () => {
    if (!await checkPermission("edit")) {
      alert("You do not have permission to edit a to-do.");
      return;
    }
    const updatedTodos = todos.map((todo, i) =>
      i === editingIndex
        ? { ...todo, content: editingContent, deadline: editingDeadline, priority: editingPriority }
        : todo
    );
    setTodos(updatedTodos);
    setEditingIndex(null);
    setEditingContent("");
    setEditingDeadline("");
    setEditingPriority("low");
  };
```

**Edit:** Function to check if the individual has "edit" permission to update the specified to-do item with new details.

```jsx
const handleSaveEdit = async () => {
    if (!await checkPermission("edit")) {
      alert("You do not have permission to edit a to-do.");
      return;
    }

    const updatedTodos = todos.map((todo, index) =>
      index === editingIndex
        ? { ...todo, content: editingContent, deadline: editingDeadline, priority: editingPriority }
        : todo
    );
    setTodos(updatedTodos);
    setEditingIndex(null);
    setEditingContent("");
    setEditingDeadline("");
    setEditingPriority("low");
  };
```

**Update:** Function to check if the individual has "update" permission, toggles the "done" status of the specified to-do item.

```jsx
const handleToggleDone = async (index) => {
    if (!await checkPermission("update")) {
      alert("You do not have permission to update a to-do.");
      return;
    }
    const updatedTodos = todos.map((todo, i) =>
      i === index ? { ...todo, done: !todo.done } : todo
    );
    setTodos(updatedTodos);
  };
```

**Delete:** Function to check if the individual has "delete" permission and removes the specified to-do item from the list.

```jsx
 const handleDeleteTodo = async (index) => {
    if (!await checkPermission("delete")) {
      alert("You do not have permission to delete a to-do.");
      return;
    }
    const updatedTodos = todos.filter((_, i) => i !== index);
    setTodos(updatedTodos);
  };
```

We have added all the required actions to restrict individuals by defining functions to create, edit, update, and delete items from our Todo app based on defined roles.

With that, our Simple Todo Application with Next.js and Permit is complete 🥳.

You can start the application by running

```jsx
npm run dev
```

This will spin up our Next.js application on [http://localhost:3000](http://localhost:3000).

## **Demo of our Todo Application**

### **Demo of an individual with user permissions**

In this demonstration, we will be showing how our users can only read Todos but cannot edit or delete them :

* **Read Todos:** The individual with a user role can read the Todos.
    
* **Restricted from Editing Blogs:** The user cannot modify existing Todos.
    
* **Restricted from Deleting Todos:** The user does not have permission to delete Todos.
    

%[https://youtu.be/03kBVFMcv8A] 

### Updating our User to Admin

To change the user's role from User to Admin, navigate to the [Permit.io](http://Permit.io) dashboard and update the user's role assignment as shown below:

![Updating our User to Admin](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/zvngnfna26hy4kr6lh6a.png align="left")

### Demo of Admin user with all the Permissions

After updating the role to Admin, the user now has full permissions within the application. As an Admin, the user can:

* Update Todos : Modify existing Todos.
    
* Delete Todos: Remove Todos from the list.
    
* Read Todos: Read existing Todos.
    

The following video demonstrates the enhanced capabilities of the Admin role:

%[https://youtu.be/hGlxLKmZ3qk] 

That’s how easy it is to implement RBAC policies in any application with [permit.io](http://permit.io).

We can also see a trail of requests from the Audit Log section in the [Permit.io](http://Permit.io) Dashboard, as shown below:

![Audit Log](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/tux24cdlvxeqi94ub4im.png align="left")

## Conclusion

In this tutorial, we explored how to set up and [configure RBAC in a Next.js application](https://www.permit.io/blog/how-to-add-rbac-in-nextjs) with [Permit.io](http://Permit.io) to control user access based on their roles.

Now that you have implemented RBAC into your application, you can improve your applications' security by leveraging it to real use cases in your application.

What if you want a more granular level of control than user roles but user-detailed identity?

For that and more, we recommend you continue reading our learning materials, such as [the difference between RBAC and ABAC](https://www.permit.io/blog/rbac-vs-abac), and [adding ABAC to your application with](https://www.permit.io/abac) [Permit.io](http://Permit.io)

Want to learn more about implementing authorization? Got questions? Reach out to us in our [Slack community](https://io.permit.io/se-slack).
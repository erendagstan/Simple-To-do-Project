# Simple-To-do-Project
<ul>
<li>Project Name:
  <ul>
    <li><a href="https://m7sm4-2iaaa-aaaab-qabra-cai.raw.ic0.app/?tag=2534525012"><strong>Simple To-Do App</strong></a></li>
  </ul>
</li>
<li>About me:
<ul>
  <li>
    LinkedIn: https://www.linkedin.com/in/mert-eren-dagistan-06eu/
  </li>
    <li>
    GitHub: https://github.com/erendagstan
  </li>
    <li>
    Kaggle: https://www.kaggle.com/merterendagistan
  </li>
    <li>
    Medium: https://medium.com/@dagstaneren
  </li>
</ul>

<li>Project Description: <ul><li>Simple To-Do is a decentralized to-do list application built on the Internet Computer platform using the Motoko programming language. This project aims to demonstrate the capabilities of Web3 technologies for creating decentralized applications (dApps). Users can add tasks, mark them as completed, and clear completed tasks from the to-do list.</li></ul>

<li>Smart Contract Address
<ul>
<li>mytki-xqaaa-aaaab-qabrq-cai</li>
<li>https://m7sm4-2iaaa-aaaab-qabra-cai.raw.ic0.app/?tag=2534525012</li>
</ul>
<li>Installation Prerequisites
  
To run the Simple To-Do application locally, ensure you have the following prerequisites installed:</p>

<ul>
  <li>DFINITY Canister SDK</li>
  <li>Motoko SDK</li>
  <li>Internet Computer Simulator or access to the Internet Computer platform</li>
</ul>

Follow the instructions provided in the project's documentation for deployment and usage.

<h2>Getting Started</h2>

<ol>
  <li><strong>Clone Repository:</strong></li>
  <pre><code>git clone &lt;repository_url&gt;</code></pre>

  <li><strong>Navigate to Project Directory:</strong></li>
  <pre><code>cd simple-to-do</code></pre>

  <li><strong>Install Dependencies:</strong></li>
  <pre><code>npm install</code></pre>

  <li><strong>Deploy Smart Contract:</strong></li>
  <pre><code>dfx deploy</code></pre>

  <li><strong>Access the Application:</strong></li>
  <p>Once deployed, access the Simple To-Do application through your web browser using the provided URL or interface.</p>
</ol>

Usage
Add new tasks by entering task descriptions and clicking the 'Add' button.
Mark tasks as completed by clicking the checkbox next to each task.
Clear completed tasks by clicking the 'Clear Completed' button.
</ul>
<hr>
<h3>Main.mo</h3>
<hr>

```
import Map "mo:base/HashMap";<br>
import Hash "mo:base/Hash";<br>
import Nat "mo:base/Nat";<br>
import Iter "mo:base/Iter";<br>
import Text "mo:base/Text";<br>,

// Define the actor -> canister
actor Assistant {

  type ToDo = {
    description: Text;
    completed: Bool;
  };

  func natHash(n: Nat) : Hash.Hash {
    Text.hash(Nat.toText(n)) // return Text.hash(Nat.toText(n));
  };

  var todos = Map.HashMap<Nat, ToDo>(0, Nat.equal, natHash); // let -> immutable, var -> mutable
  var nextId : Nat = 0;

  public query func getTodos() : async [ToDo] {
    Iter.toArray(todos.vals());
  };

  // Returns the ID that was given to the ToDo item
  public func addTodo(description:Text) : async Nat {
    let id = nextId;
    todos.put(id, {description = description; completed = false });
    nextId += 1;
    id // return id
  };

  public func completeTodo(id:Nat): async() {
    ignore do ? {
      let description = todos.get(id)!.description;
      todos.put(id, {description; completed = true});
    }
  };


  public query func showTodos() : async Text {
    var output : Text = "\n___TO-DOs___";
    for (todo : ToDo in todos.vals()) {
      output #= "\n" # todo.description;
      if (todo.completed) { output #= " ✔"; };
    };
    output # "\n"
  };

public func clearCompleted() : async () {
    todos := Map.mapFilter<Nat, ToDo, ToDo>(todos, Nat.equal, natHash, 
              func(_, todo) { if (todo.completed) null else ?todo });
  };

};

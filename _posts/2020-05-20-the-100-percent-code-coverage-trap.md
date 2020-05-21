---
layout: post
title: "The 100% code coverage trap"
tags: [""]
---

Maybe one of the most appreciate metrics for programmers is the code coverage. This percentage determines how much of the testing coverage your project has. A bit of programmer really cares about what this number means, let’s talk about it.

### What does it metric say about your project?

First of all, it is only a measure that does not ensure that your software really is well-tested. Let me show an example to you:

```ruby
describe 'GET #index format: json' do
  it 'returns http success' do
    get :index, params: {}, format: 'json'
    expect(response).to have_http_status(:success)
  end
end
```

After running this test using the command `RSpec`, the controller action `index` will be checked as covered by `simplecov` gem. But, let’s imagine that our controller is like this:

```ruby
def index
  @clients = Client.active_only
end
```

Ok, our test ensures that the controller action returns some Http status 2xx. But, when you look closer to this action, it’s easy to realize that although this code chuck is coverage our first test alone doesn’t ensure its correct work.

### How can I ensure the correct work?

You have to keep in mind that there are two different layers to test in this case: the controller and the model.

To test the model we need to think in some test cases like:

* There aren't active clients, so the return should be empty
* There are active clients and then return should be them
* There are active and inactive clients and the return should be only the actives

```ruby
describe '.active_only' do
  context "when there aren't clients" do
    
    it { expect(described_class.active_only).to be_empty }

  end

  context "when there are active and inactive" do
    let!(:inactives) { create_list(:client, 3, active: false) }
    let!(:actives) { create_list(:client, 4, active: true) }

    it 'returns only actives' do
      expect(described_class.active_only).to eq(actives)
    end
  end

  context "when there are only inactives" do
    let!(:inactives) { create_list(:client, 3, active: false) }

    it { expect(described_class.active_only).to be_empty }
  end
end
```

How about the controller? How we can ensure it correctly work?

In the controller, we don’t need this behavior again, but the assign of the instance variable @clients.

```ruby
describe 'GET #index format: json' do
  let!(:clients) { create_list(:clients, 3, active: true) }

  it 'returns http success' do
    get :index, params: {}, format: 'json'
    expect(response).to have_http_status(:success)
  end

  it 'assings @clients' do
    get :index, params: {}, format: 'json'
    expect(assigns(:clients)).to eq(clients)
  end
end
```

### Conclusion

When we are working with TDD the most important is to ensure if our classes behave like the expected and express the requirements on the test cases. Although, the code coverage could be an excellent way to looks at our project’s health.

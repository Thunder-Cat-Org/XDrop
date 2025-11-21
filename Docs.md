## TOKEN DROP

claimed: used as claimed[pool_id, address]
claimed: is the amount a whitelisted address has claimed from a given pool.

```python
claimed = Hash(default_value=0)
```

deposits: used as deposits[pool_owner, token]
deposits: is the amount of tokens a pool owner has deposited into a pool.

```python
deposits = Hash(default_value=0)
```

allocated: used as allocated[pool_id, address]
allocated: is the number of tokens allocated to an address in a given pool.

```python
allocated = Hash()
```

owner: pool_owner[pool_id] -> owner
owner: is the address of a pool creator.

```python
pool_owner = Hash()
```

pool_token: pool_token[pool_id] -> token contract name
pool_token: is a token that is deposited in a given pool.

```python
pool_token = Hash()
```

pool_balance: pool_balance[pool_id] -> float
pool_balance: is the total amount of a pool token remaining.

```python
pool_balance = Hash(default_value=0)
```

pool_mod: pool_mode[pool_id] -> "whitelist" or "open"
pool_mod: is the mode of a poool. "whitelist" means the pool tokens can only be claimed by whitelisted addresses.
"open"means its open to every xian address.

```python
pool_mode = Hash()
```

pool_blacklist: # pool_blacklist[(pool_id, addr)] = True/False
pool_blacklist: is a list of addresses blacklisted in a given pool.

```python
pool_blacklist = Hash(default_value=False)
```

open_pool_limit: limit a single address can claim from an open pool. Limit set by pool owner.

```python
open_pool_limit = Hash(default_value=None)
```

### Create pool

````python
def create_pool(pool_name: str, token_contract: str, mode: str):
    ```
````

Pool name is chosen by the pool creator. Pool name must be unique per creator.
mode: "whitelist" or "open"
pool_id will be ctx.caller + ":" + pool_name

### Deposit to pool

```python
def deposit_to_pool(pool_id: str, amount: float):

```

Pool owner deposits tokens into their pool (caller must be pool owner).
Requires the token contract to support transfer_from approved by owner.

### Set allocation

````python
def set_allocation(pool_id: str, allocations: list):
    ```
````

Pool owner sets allocations for addresses for this pool.
allocations: list of dicts {'address': addr_str, 'amount': float}

### Pool owner blacklist

````python
def blacklist_addresses(pool_id: str, addresses: list):
    ```
````

Pool owner blacklists addresses for this pool.

### Remove from blacklist

````python
def remove_from_blacklist(pool_id: str, addresses: list):
    ```
````

Pool owner removes addresses from blacklist for this pool.

### Check blacklist status

```python
def is_address_blacklisted(pool_id: str, address: str):
```

Checks blacklist status in a given pool.

### Get allocation in a pool

````python
def get_allocation(pool_id: str, address: str):
    ```
````

Get allocation from a pool.

### Get claimed

```python
def get_claimed(pool_id: str, address: str):
```

Get number of tokens claimed by an address from a pool.

### Get Pool balance

```python
def get_pool_balance(pool_id: str):

```

Get pool balance.

## Claim

````python
def claim(pool_id: str, amount: float, to: str):
    ```
````

Claim from pool. Behavior depends on pool_mode: - "whitelist": must have an allocated amount and not exceed allocation - "open": any non-blacklisted address can claim until pool_balance is 0

### Withdraw from pool

```python
def withdraw_from_pool(pool_id: str, amount: float):
```

Pool owner withdraws unclaimed tokens from pool back to themselves.

### Set open pool limit

```python
def set_open_pool_limit(pool_id: str, limit: float):
```

Limit a single address can claim from an open pool. Limit is set by pool owner.

### Transfer operator

````python
def transfer_operator(new_operator: str):
    ```
````

Transfer contract ownership to a new operator.

### Revoke mutability

````python
def revoke_mutability():
    ```
````

Make contract unchangeable forever.

### Get pool stats

```python
def get_allocation_stats(pool_id: str):
```

Returns

```json
{
  "pool_id": "owner:pool69",
  "total_allocated": 200,
  "pool_balance": 10,
  "status": "underfunded"
}
```

status "underfunded": means allocated > deposits.
status "fully_funded" means allocated = deposits.
status "overfunded" means deposits > allocated.

Mostly useful for whitelist pools only.

### Get effective allocation

Get effective allocation for an address in an "open" pool (the open pool limit) or in a
"whitelist" pool (the allocated amount)

```python
def get_effective_allocation(pool_id: str, address: str):
```

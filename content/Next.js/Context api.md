```jsx:
import { useContext, createContext, useState } from 'react';
import { ReactNode } from 'react';

type User = {
id: string;
name: string;
email: string;
};

type UserContextProps = {
user: User;
setUser: React.Dispatch<React.SetStateAction<User>>;
};
const UserContext = createContext<UserContextProps | null>(null);
export function UserProvider({
children,
initialData,
}: {
children: ReactNode;
initialData: User;

}) {

const [user, setUser] = useState<User>(initialData);
return (
<UserContext.Provider value={{ user, setUser }}>
{children}
</UserContext.Provider>
);
}

export function useAppContext() {
const context = useContext(UserContext);
if (!context) {
throw new Error('useAppContext must be used within a UserProvider');
}
return context;
}




const initialUserData = {
    id: '123',
    name: 'John Doe',
    email: 'john@example.com',
  };
  <UserProvider initialData={initialUserData}>
        <PhoneTopUpContent />
      </UserProvider>
      
      Trong PhoneTopUpContent
      const { user, setUser } = useAppContext();
```




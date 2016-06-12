# the set up
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base


engine = create_engine('postgresql://ubuntu:thinkful@localhost:5432/tbay')
Session = sessionmaker(bind=engine)
session = Session()
Base = declarative_base()

pattern for class:
from datetime import datetime

from sqlalchemy import Column, Integer, String, DateTime

class Item(Base):
    __tablename__ = "items"

    id = Column(Integer, primary_key=True)
    name = Column(String, nullable=False)
    description = Column(String)
    start_time = Column(DateTime, default=datetime.utcnow)
    
the following command creates the tables and ignores ones that have already been created:
Base.metadata.create_all(engine)

# querying

Returns a list of all of the user objects
Note that user objects won't display very prettily by default -
you'll see their type (User) and their internal identifiers.
*session.query(User).all() # Returns a list of all of the user objects*

Returns the first user
*session.query(User).first()*

Finds the user with the primary key equal to 1
*session.query(User).get(1)*

Returns a list of all of the usernames in ascending order
*session.query(User.username).order_by(User.username).all()*

Returns the description of all of the basesballs
*session.query(Item.description).filter(Item.name == "baseball").all()*

Return the item id and description for all baseballs which were created in the past.  Remember to import the datetime object: from datetime import datetime
*session.query(Item.id, Item.description).filter(Item.name == "baseball", Item.start_time < datetime.utcnow()).all()*

# updating

user = session.query(User).first()
user.username = "solange"
session.commit()

# deleting
user = session.query(User).first()
session.delete(user)
session.commit()

# relationships

## one-to-one
from sqlalchemy.orm import relationship

class Person(Base):
    __tablename__ = 'person'
    id = Column(Integer, primary_key=True)
    name = Column(String, nullable=False)

    passport = relationship("Passport", uselist=False, backref="owner")

class Passport(Base):
    __tablename__ = 'passport'
    id = Column(Integer, primary_key=True)
    issue_date = Column(Date, nullable=False, default=datetime.utcnow)

    owner_id = Column(Integer, ForeignKey('person.id'), nullable=False)

beyonce = Person(name="Beyonce Knowles")
passport = Passport()
beyonce.passport = passport

session.add(beyonce)
session.commit()

print(beyonce.passport.issue_date)
print(passport.owner.name)

*the uselist=False is what creates the one-one-one relationship*
*the backref="owner" allows us to access the passport through the person but also the person through the passport*

## one-to-many
*a one-to-many relationship is created in the same way as a one-to-one except that the uselist=False is omitted*

fender = Manufacturer(name="Fender")
strat = Guitar(name="Stratocaster", manufacturer=fender)
tele = Guitar(name="Telecaster")
fender.guitars.append(tele)

session.add_all([fender, strat, tele])
session.commit()

for guitar in fender.guitars:
    print(guitar.name)
print(tele.manufacturer.name)

*setting the manufacturer=fender property or appending the object to the parent's guitar property like with the Telecaster has the same end result.*

## many-to-many
*many-to-many works like two one-to-many relationships with an intermediate table; the intermediate holds foreign keys for both sides of the many-to-many relationship*

*notice new import from sqlalchemy, Table*

from sqlalchemy import Table, Column, Integer, String, ForeignKey
from sqlalchemy.orm import relationship

pizza_topping_table = Table('pizza_topping_association', Base.metadata,
    Column('pizza_id', Integer, ForeignKey('pizza.id')),
    Column('topping_id', Integer, ForeignKey('topping.id'))
)

class Pizza(Base):
    __tablename__ = 'pizza'
    id = Column(Integer, primary_key=True)
    name = Column(String, nullable=False)
    toppings = relationship("Topping", secondary="pizza_topping_association",
                            backref="pizzas")
                            
*secondary argument here will cause SQLAlchemy to lookup related values in the intermedate table, 'pizza_topping_association'.*

class Topping(Base):
    __tablename__ = 'topping'
    id = Column(Integer, primary_key=True)
    name = Column(String, nullable=False)

peppers = Topping(name="Peppers")
garlic = Topping(name="Garlic")
chilli = Topping(name="Chilli")

spicy_pepper = Pizza(name="Spicy Pepper")
spicy_pepper.toppings = [peppers, chilli]

vampire_weekend = Pizza(name="Vampire Weekend")
vampire_weekend.toppings = [garlic, chilli]


session.add_all([garlic, peppers, chilli, spicy_pepper, vampire_weekend])
session.commit()

for topping in vampire_weekend.toppings:
    print(topping.name)

for pizza in chilli.pizzas:
    print(pizza.name)
    
*both pizza.toppings and topping.pizzas are lists*
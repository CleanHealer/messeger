'use client'

import { useEffect, useState } from 'react';
import { useAuthState } from 'react-firebase-hooks/auth';
import { signInWithPopup } from 'firebase/auth';
import { auth, googleProvider, db } from '@/lib/firebase';
import { doc, setDoc, getDoc, updateDoc, serverTimestamp } from 'firebase/firestore';
import { handleFirestoreError, OperationType } from '@/lib/firestore-errors';
import { Button } from '@/components/ui/button';
import { LogIn, MessageSquare, Zap } from 'lucide-react';
import Messenger from '@/components/Messenger';
import { motion } from 'motion/react';

export default function Home() {
  const [user, loading, error] = useAuthState(auth);
  const [isInitializing, setIsInitializing] = useState(false);

  useEffect(() => {
    async function initUser() {
      if (user) {
        setIsInitializing(true);
        try {
          // Check if user profile exists
          const userRef = doc(db, 'users', user.uid);
          const userSnap = await getDoc(userRef).catch(e => {
            handleFirestoreError(e, OperationType.GET, `users/${user.uid}`);
            return null;
          });

          if (userSnap && !userSnap.exists()) {
            await setDoc(userRef, {
              email: user.email || '',
              displayName: user.displayName || 'Anonymous',
              photoURL: user.photoURL || '',
              isOnline: true,
              createdAt: serverTimestamp()
            }).catch(e => handleFirestoreError(e, OperationType.CREATE, `users/${user.uid}`));
          } else if (userSnap?.exists()) {
             await updateDoc(userRef, {
               isOnline: true,
               lastSeen: serverTimestamp()
             }).catch(e => handleFirestoreError(e, OperationType.UPDATE, `users/${user.uid}`));
          }
        } finally {
          setIsInitializing(false);
        }
      }
    }
    initUser();
  }, [user]);

  const login = () => {
    signInWithPopup(auth, googleProvider).catch((e) => {
      console.error('Error signing in', e);
    });
  };

  if (loading || isInitializing) {
    return (
      <div className="flex h-screen w-full items-center justify-center bg-[#0F0F12]">
        <motion.div 
          animate={{ scale: [1, 1.1, 1], opacity: [0.5, 1, 0.5] }}
          transition={{ repeat: Infinity, duration: 1.5 }}
          className="flex flex-col items-center"
        >
           <MessageSquare className="h-16 w-16 text-blue-500 mb-4" />
           <div className="text-white font-medium tracking-wide">Загрузка...</div>
        </motion.div>
      </div>
    );
  }

  if (!user) {
    return (
      <div className="relative flex h-screen w-full overflow-hidden bg-[#0F0F12]">
        <div className="absolute inset-0 bg-[url('https://images.unsplash.com/photo-1550745165-9bc0b252726f?q=80&w=2500&auto=format&fit=crop')] bg-cover bg-center opacity-20" />
        <div className="absolute inset-0 bg-gradient-to-t from-[#0F0F12] via-transparent to-[#0F0F12]" />

        <div className="relative flex h-full w-full items-center justify-center p-4">
          <motion.div 
            initial={{ opacity: 0, y: 30 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 0.8, ease: "easeOut" }}
            className="max-w-md w-full backdrop-blur-xl bg-[#1F1F23]/80 rounded-3xl shadow-2xl flex flex-col items-center p-10 text-center border border-white/5 relative overflow-hidden"
          >
            <motion.div 
              initial={{ scale: 0.8 }}
              animate={{ scale: 1 }}
              transition={{ delay: 0.2, type: "spring", stiffness: 200 }}
              className="bg-blue-600 p-5 rounded-2xl mb-8 shadow-xl shadow-blue-500/20"
            >
              <MessageSquare className="h-12 w-12 text-white" />
            </motion.div>

            <h1 className="text-4xl font-bold tracking-tight text-white mb-3">
              Общайтесь <span className="text-blue-400">свободно</span>
            </h1>
            <p className="text-[#A1A1AA] mb-10 font-medium">Ваш новый опыт связи. Современный и безопасный мессенджер.</p>
            
            <Button 
              onClick={login} 
              size="lg" 
              className="w-full h-14 text-base font-semibold bg-white text-black hover:bg-gray-100 rounded-xl transition-all duration-300 relative group overflow-hidden border-none"
            >
              <span className="relative z-10 flex items-center justify-center">
                <LogIn className="mr-3 h-5 w-5" />
                Войти через Google
              </span>
            </Button>
          </motion.div>
        </div>
      </div>
    );
  }

  return <Messenger user={user} />;
}
